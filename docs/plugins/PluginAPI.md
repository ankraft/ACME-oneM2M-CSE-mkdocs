
# Plugin API & Lifecycle

Plugins in the ACME CSE follow the same lifecycle as the CSE itself, consisting of several phases
that are shown in the following diagram.  

```mermaid
---
title: Plugin Lifecycle
---
stateDiagram
    direction LR

	state Initializing {
		[*] --> loaded
		loaded --> initialized:@init
		initialized:<i>initialized
		configured:<i>configured
		validated:<i>validated
		resolved:<i>resolved

		initialized --> configured:@configure
		configured --> validated:@validate
		validated --> resolved:@on_resolved

	}
	
	state Running {
		started1:<i>started
		restarting1:<i>restarted

		started1 --> restarting1:@restart
		restarting1 --> started1
	}

	state Paused {
		pausing:<i>pausing
		unpaused:unpaused
		restarting2:<i>restarted

		pausing --> unpaused:@unpause
		pausing --> restarting2:@restart
	}

	state Stopped {
		[*] --> stopped:@stop
		stopped:<i>stopped
	}

	state Finalizing {
		[*] --> unresolved:@on_unresolved
		unresolved --> finalized:@finish
		unresolved:<i>unresolved
		finalized:<i>finalized
		unloaded:<i>Unloaded
	}


	
	[*] --> Initializing
	Initializing --> Running:@start
	Running --> Paused:@pause
	Paused --> Running
	Paused --> Stopped
	Running --> Stopped
	Stopped --> Running
	Stopped --> Finalizing
	finalized --> unloaded
	Finalizing --> [*]

```


The phases of the plugin lifecycle are implemented as methods within the plugin class, each decorated with specific decorators provided by the `PluginManager`. A plugin can implement none, some, or all of the lifecycle methods.

Plugins run in the same thread as the main CSE process, so be careful not to block the main thread within plugin methods. If you need to perform long-running tasks, consider using separate threads or asynchronous programming techniques.


## Plugin Class Decorators

### @plugin

A single class within the plugin module must be decorated with `@PluginManager.plugin` to indicate that it is the plugin class. This class will be instantiated by the `PluginManager` when the plugin is loaded. Within this class, you can define methods that are decorated to hook into the various lifecycle events of the plugin.

The class can have any name, but it is common practice to name it after the plugin itself (e.g., `HelloWorldPlugin` for a plugin named `HelloWorld`). The class can also have an `__init__` method for any necessary initialization, but keep in mind that the lifecycle methods will be called after the class is instantiated, so any setup that depends on the CSE's state should be done in the appropriate lifecycle method rather than in `__init__`.

#### Decorator Arguments

The `@PluginManager.plugin` class decorator can also take optional parameters, which are passed as keyword arguments. 

- `property`: A string that specifies the name of a property on the `PluginManager` through which the instantiated plugin class can be accessed. If this parameter is not provided, the plugin class will not be directly accessible via a property on the `PluginManager`.  
	If no property name is provided, the plugin class will not be directly accessible via a property on the `PluginManager`, but it can still be accessed through the plugin information as described in the [PluginManager documentation](PluginManager.md#accessing-plugin-information-and-modules). 
- `priority`: An integer that specifies the priority of the plugin class instantiation. Plugin classes with a lower priority value are instantiated before those with a higher priority value. The default priority is `50`. This can be useful if your plugin class depends on another plugin class being instantiated first.
- `tags`: A list of strings that can be used to tag the plugin class with specific keywords. This can be useful for categorizing plugins or for filtering plugins based on their tags at runtime.
- `noRestartWhilePaused`: Normally, plugins can be restarted while paused. This flag indicates that the plugin should not be restarted while in paused state. This can be useful for plugins that maintain state that should not be reset while paused.

#### Example 

```python title="Plugin Class Decorator with Parameters"
@PluginManager.plugin(property='my_plugin', 
					  priority=10, 
					  tags=['example', 'test'], 
					  noRestartWhilePaused=True)
class MyPlugin:
	...
```

### @requires

The `@PluginManager.requires` class decorator can be used to specify that a plugin depends on another plugin. This ensures that the dependent plugin is resolved before the plugin is started. If the required plugin is not available, the whole startup process will fail with an error. This behaviour can be changed by setting the optional argument `required` to `False`, which will cause the plugin to be processed as usual, though the dependency is not resolved.

One important aspect of using the `@PluginManager.requires` class decorator is that the dependency plugin's instance is injected into the decorated class as an attribute with the same name as the argument in the `@requires` decorator. If the dependend plugin becomes unavailable, the attribute will be set to `None`.

See the following examples for more details.

The `@requires` decorator can be used multiple times together with the `@plugin` decorator for the same class, or even without the `@plugin` decorator for non-plugin classes. 

#### Using `@requires` with a Non-Plugin Class

In case a class is not a plugin itself, the `@requires` decorator can be used to express dependencies to plugins and to have the plugin instances automatically injected into the class. This can be useful for classes that depend on the functionality provided by a plugin and want to access plugin instances directly.

A plugin or non-plugin class can monitor the availability of its dependencies by using the [`@on_resolved`](#on_resolved) and [`@on_unresolved`](#on_unresolved) decorators, which are called when all dependencies are resolved and when any dependency becomes unresolved, respectively. This allows the class to react to changes in the availability of its dependencies at runtime.


#### Decorator Arguments

The `@PluginManager.requires` class decorator takes keyword arguments: 

- `<attribute name> = <plugin name>`: the keyword name is the name of the attribute through which the plugin instance will be injected into the decorated class, and the value is the name of the required plugin. The plugin name must match the plugin module name (i.e., the filename without the `.py` extension).  
	This is a required argument, and can occur multiple times to specify multiple dependencies.
- `required`: A boolean that indicates whether the dependency is required for the plugin to function. If set to `True`, the whole startup process will fail with an error if the required plugin is not available. If set to `False`, the plugin will be processed as usual, though the dependency is not resolved. The default value is `True`.


#### Example: Plugin Class with Dependency

```python title="Plugin Class Decorator with Dependency"
@PluginManager.plugin	                                                    # Mark the class as a plugin class
@PluginManager.requires(dependency_plugin='acme.plugins.DependencyPlugin')	# Plugin depends on 'DependencyPlugin'
class MyPlugin:

	def aFunction(self):
		# Access the instance of the dependency plugin via the injected attribute
		self.dependency_plugin.someFunction()
```


#### Example: Non-Plugin Class with Dependency

```python title="Non-Plugin Class Decorator with Dependency"
@PluginManager.requires(dependency_plugin='acme.plugins.DependencyPlugin')	# Class depends on 'DependencyPlugin'
class MyClass:

	def aFunction(self):
		# Access the instance of the dependency plugin via the injected attribute
		self.dependency_plugin.someFunction()
```


#### Example: Optional Dependency on Multiple Plugins

```python title="Optional Dependency on Multiple Plugins"
@PluginManager.requires(dependency_plugin1='acme.plugins.DependencyPlugin1', 
						dependency_plugin2='acme.plugins.DependencyPlugin2',
						required=False)	# Optional
class MyClass:

	# Adding attributes for easier checking of whether the dependencies are resolved or not
	dependency_plugin1 = None
	dependency_plugin2 = None

	def aFunction(self):
		# The following will only call someFunction() if the dependencies are resolved,
		# otherwise it will do nothing, as the attributes will be None
		self.dependency_plugin1 and self.dependency_plugin1.someFunction()	
		self.dependency_plugin2 and self.dependency_plugin2.someFunction()
```


## Decorators for Lifecycle Methods

### @init - Initialization

The plugin is loaded and initialized when the CSE starts. The `@PluginManager.init` decorated method is called during this phase.

The signature of the `@PluginManager.init` method is as follows:

```python title="Example: Plugin Initialization Decorator"
@PluginManager.init
def init(self) -> None:
    ...
```

### @configure - Configuration

The plugin can read configuration settings from the CSE's configuration file during this phase. The `@PluginManager.configure` decorated method is called during this phase with the `acme.runtime.Configuration.Configuration` object as an argument. This method should raise an exception if required configuration settings are missing or invalid.

The signature of the `@PluginManager.configure` method is as follows:

```python title="Example: Plugin Configuration Decorator"
@PluginManager.configure
def configure(self, config: Configuration) -> None:
    ...
```


### @validate - Validation

The plugin can validate its configuration settings during this phase. The `@PluginManager.validate` decorated method is called during this phase with the `acme.runtime.Configuration.Configuration` object as an argument, and therefore has access to ACME's configuration settings. This method should raise an exception if the configuration is invalid.

This phase occurs after configuration and before activation. The plugin can use this phase to ensure that all necessary configuration settings are present and inline with other configuration settings.

The signature of the `@PluginManager.validate` method should be as follows:

```python title="Example: Plugin Validation Decorator"
@PluginManager.validate
def validate(self, config: Configuration) -> None:
    ...
```


### @start - Running

The plugin becomes active and starts performing its intended functions. The `@PluginManager.start` decorated method is called during this phase.

The signature of the `@PluginManager.start` method is as follows:

```python title="Example: Plugin Start Decorator"
@PluginManager.start
def start(self) -> None:
    ...
```


### @pause - Paused

The plugin is paused and temporarily stops performing its functions. The `@PluginManager.pause` decorated method is called during this phase.

The signature of the `@PluginManager.pause` method is as follows:

```python title="Example: Plugin Pause Decorator"
@PluginManager.pause
def pause(self) -> None:
    ...
```


### @unpause - Unpaused

The plugin is unpaused and resumes performing its functions. The `@PluginManager.unpause` decorated method is called during this phase.

!!! note "Unpause not called when Restarting while Paused"
	If the plugin is in *paused* state and the plugin is restarted while paused, the `@PluginManager.restart` method will be called, but the `@PluginManager.unpause` method will not be called.
	

The signature of the `@PluginManager.unpause` method is as follows:

```python title="Example: Plugin Unpause Decorator"
@PluginManager.unpause
def unpause(self) -> None:
	...
```


### @stop - Stopped

The plugin is deactivated and stops performing its functions. The `@PluginManager.stop` decorated method is called during this phase.

If the plugin is in *paused* state and the `@plugin` class decorator has the `noRestartWhilePaused` flag set to `True`, the plugin will not be restarted while paused.

The signature of the `@PluginManager.stop` method is as follows:

```python title="Example: Plugin Stop Decorator"
@PluginManager.stop
def stop(self) -> None:
    ...
```

### @restart - Restarting

If the CSE is restarted internally, the plugin's `@PluginManager.restart` decorated method is called. This allows the plugin to reinitialize any state or resources as needed. After this method is called, the plugin is considered to be in the running state again. However, neither the `@PluginManager.start` nor `@PluginManager.stop` methods are called during a restart.

!!! note "Not restarting while Paused"
	If the plugin is in *paused* state and the `@plugin` class decorator has the `noRestartWhilePaused` flag set to `True`, the plugin will not be restarted while paused, and therefore the `@PluginManager.restart` method will not be called. In this case, the plugin will remain in the paused state until it is unpaused, at which point the `@PluginManager.unpause` method will be called to resume its functions.

The signature of the `@PluginManager.restart` method is as follows:

```python title="Example: Plugin Restart Decorator"
@PluginManager.restart
def restart(self) -> None:
    ...
```

### @finish - Finalization

The plugin is finalized and cleaned up when the CSE shuts down. The `@PluginManager.finish` decorated method is called during this phase. This is the last phase of the plugin's lifecycle.

The signature of the `@PluginManager.finish` method is as follows:

```python title="Example: Plugin Finalization Decorator"
@PluginManager.finish
def finish(self) -> None:
    ...
```


### @on_resolved

The `@PluginManager.on_resolved` decorated method is called when all dependencies of the plugin are resolved and the plugin is ready to be started. This can be used to perform any necessary setup that depends on the availability of the plugin's dependencies.

The decorated method receives a list of `Dependency` objects as an argument, which represent the resolved and unresolved dependencies of the plugin or class. Each `Dependency` object contains information about the dependency, including its name, the instance name, and whether it is required and was resolved.

The signature of the `@PluginManager.on_resolved` method is as follows:

```python title="Example: Plugin Resolved Decorator"
@PluginManager.on_resolved
def on_resolved_handler(self, dependencies:list[Dependency]) -> None:
	...
```


### @on_unresolved

The `@PluginManager.on_unresolved` decorated method is called when any dependency of the plugin becomes unresolved. This can be used to perform any necessary cleanup or state management when a dependency becomes unavailable at runtime.

The decorated method receives a list of `Dependency` objects as an argument, which represent the resolved and unresolved dependencies of the plugin or class. Each `Dependency` object contains information about the dependency, including its name, the instance name, and whether it is required and was resolved.

The signature of the `@PluginManager.on_unresolved` method is as follows:

```python title="Example: Plugin Unresolved Decorator"
@PluginManager.on_unresolved
def on_unresolved_handler(self, dependencies:list[Dependency]) -> None:
	...
```


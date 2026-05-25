# The PluginManager

The [PluginManager](https://api.acmecse.net/acmecse.helpers.PluginManager.PluginManager.html){target="_new"} is the runtime component of the ACME CSE that is responsible for managing the lifecycle of plugins. It provides decorators, methods and properties that plugins can use to hook into the CSE's lifecycle events. It is loaded and run as part of the CSE's startup process. Similar, it is finalized during the CSE's shutdown process.

The [PluginManager](https://api.acmecse.net/acmecse.helpers.PluginManager.PluginManager.html){target="_new"} offers the possibility to access loaded plugins and their assigned plugin classes at runtime. This can be useful for plugins that need to interact with other plugins or for debugging purposes.

How to access loaded plugins and their plugin classes is described in the following sections.



## Accessing Plugin Information and Modules

The [PluginManager](https://api.acmecse.net/acmecse.helpers.PluginManager.PluginManager.html){target="_new"} provides a convenient way to access the runtime information of loaded plugins. This includes the ability to get the Python module, but also other information such as a plugin's name and its associated plugin class. This can be useful for introspection or for dynamically loading additional resources from the plugin's module. The [PluginManager](https://api.acmecse.net/acmecse.helpers.PluginManager.PluginManager.html){target="_new"} is a singleton and can therefore be accessed from anywhere in the codebase. 

Plugin information can be accessed through the `pluginManager.<PluginName>` property, where `<PluginName>` is the name of the plugin module (i.e., the filename without the `.py` extension).


```python title="Getting Plugin Python Module"
from acmecse.runtime.PluginSupport import *

# Get the singleton instance of the PluginManager
pluginManager = PluginManager()	

# Get the module for a specific plugin
hello_world_info = pluginManager.HelloWorld
hello_world_module = hello_world_info.module
```

The `hello_world_module` variable will now contain the Python module object for the `HelloWorld` plugin, allowing access to its Python attributes, classes, and functions. 

The plugin information contains additional properties, including the current plugin state, pointers to the decorated lifecycle methods, and the instantiated plugin class.


## Accessing the Plugin Class

The [PluginManager](https://api.acmecse.net/acmecse.helpers.PluginManager.PluginManager.html){target="_new"} always instantiates the plugin's class that is decorated with [@plugin](https://api.acmecse.net/acmecse.helpers.PluginManager.html#plugin){target="_new"} when the plugin is loaded. By default it is only accessible via Plugin information, but it is not directly exposed for access.

However, it is possible to pass a name for a property in the `@PluginManager.plugin` class decorator, which will cause the instantiated plugin class to be accessible via a property on the PluginManager with that name.

For example, the `HelloWorld` plugin class can be decorated as follows:

```python title="HelloWorld.py with Named Plugin Class"
from acmecse.runtime.PluginSupport import *

@plugin(property='greetings')
class HelloWorld:
    ...
```

The instantiated plugin class can then be accessed via the `PluginManager.greetings` property:

```python title="Getting Named Plugin Class"
hello_world_instance = pluginManager.greetings
```


## Setting the Plugin Class Priority

By default, the [PluginManager](https://api.acmecse.net/acmecse.helpers.PluginManager.PluginManager.html){target="_new"} instantiates and processes plugin classes in the order they are loaded. However, one can set a priority for the plugin class instantiation using the `priority` parameter in the [@plugin](https://api.acmecse.net/acmecse.helpers.PluginManager.html#plugin){target="_new"} class decorator. This can be useful if a plugin class depends on another plugin class being instantiated first.

Plugin classes with a lower priority value are instantiated before those with a higher priority value. The default priority is `50`.

```python title="HelloWorld.py with Priority"
from acmecse.runtime.PluginSupport import *

@plugin(priority=10)
class HelloWorld:
	...
```

In this example, the `HelloWorld` plugin class will be instantiated before any other plugin classes with the default priority of `50`.

It is important to note that plugin classes with a lower priority value are instantiated first, but are stopped and finalized later, i.e. after plugin classes with a higher priority value, during the CSE shutdown or unloading process. 


## Tagging Plugin Classes

The [@plugin](https://api.acmecse.net/acmecse.helpers.PluginManager.html#plugin){target="_new"} class decorator also allows for tagging plugin classes with specific keywords using the `tags` parameter. This can be useful for categorizing plugins or for filtering plugins based on their tags at runtime.

```python title="HelloWorld.py with Tags"
from acmecse.runtime.PluginSupport import *

@plugin(tags=['greeting', 'example'])
class HelloWorld:
    ...
```

In this example, the `HelloWorld` plugin class is tagged with the keywords 'greeting' and 'example'. This information can be accessed through the plugin information for the `HelloWorld` plugin, which can be useful for filtering or categorizing plugins at runtime.

For example, the protocol binding plugins are all tagged with the keyword 'binding'.


## Getting All Plugin Information

The information of all loaded plugins can be accessed via the [plugins](https://api.acmecse.net/acmecse.helpers.PluginManager.PluginManager.html#plugins){target="_new"} property. This is a dictionary of `PluginInfo` objects, each containing information about a loaded plugin, including its name, module, state, documentation, and associated plugin class.

Usually, this is only needed for debugging or introspection purposes. One should be careful when using this property, as it provides access to all loaded plugins and their information, which can lead to unintended consequences if not used properly.

```python title="Getting Plugin Information"
from acmecse.runtime.PluginSupport import *

# Get the singleton instance of the PluginManager
pluginManager = PluginManager()

# Get a list of all loaded plugins
loaded_plugins = pluginManager.plugins
```


## Providing Non-Plugin Class Instances

The [PluginManager](https://api.acmecse.net/acmecse.helpers.PluginManager.PluginManager.html){target="_new"} also allows for providing non-plugin class instandces, that can be required by plugin classes via the [@requires](https://api.acmecse.net/acmecse.helpers.PluginManager.html#requires){target="_new"} decorator. This can be useful for providing specific functionality to plugin classes that cannot or should not be implemented as a plugin class.

This is done by the PluginManager's [provide](https://api.acmecse.net/acmecse.helpers.PluginManager.PluginManager.html#provide){target="_new`} method. This method takes a string argument that specifies the path to the provided function and the class instance itself.

```python title="Providing a Non-Plugin Class Instance"
from acmecse.runtime.PluginSupport import *

# Get the singleton instance of the PluginManager
pluginManager = PluginManager()

# Create an instance of the non-plugin class
non_plugin_instance = NonPluginClass()

# Provide the non-plugin class instance to the PluginManager
pluginManager.provide('a.path.to.non_plugin_instance', non_plugin_instance)
```

This will make the `non_plugin_instance` available for injection into plugin classes that require it via the [@requires](https://api.acmecse.net/acmecse.helpers.PluginManager.html#requires){target="_new"} decorator.

```python title="Using a Non-Plugin Class Instance in a Plugin"
from acmecse.runtime.PluginSupport import *

@requires(non_plugin_instance='a.path.to.non_plugin_instance')
class ExamplePlugin:
    def aFunction(self):
		# Access the non-plugin class instance via the injected attribute
		self.non_plugin_instance.someMethod()
```
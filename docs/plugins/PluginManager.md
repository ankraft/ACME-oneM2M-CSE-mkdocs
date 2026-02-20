# The PluginManager

The `PluginManager` is the runtime component of the ACME CSE that is responsible for managing the lifecycle of plugins. It provides decorators, methods and properties that plugins can use to hook into the CSE's lifecycle events. It is loaded and run as part of the CSE's startup process. Similar, it is finalized during the CSE's shutdown process.

The PluginManager offers the possibility to access loaded plugins and their assigned plugin classes at runtime. This can be useful for plugins that need to interact with other plugins or for debugging purposes.

How to access loaded plugins and their plugin classes is described in the following sections.


## Accessing Plugin Information and Modules

The `PluginManager` provides a convenient way to access the runtime information of loaded plugins. This includes the ability to get the Python module, but also other information such as a plugin's name and its associated plugin class. This can be useful for introspection or for dynamically loading additional resources from the plugin's module.

Plugin information can be accessed through the `PluginManager.<PluginName>` property, where `<PluginName>` is the name of the plugin module (i.e., the filename without the `.py` extension).


```python title="Getting Plugin Python Module"
from acme.runtime import PluginManager

# Get the module for a specific plugin
hello_world_info = PluginManager.HelloWorld
hello_world_module = hello_world_info.module
```

The `hello_world_module` variable will now contain the Python module object for the `HelloWorld` plugin, allowing access to its Python attributes, classes, and functions. 

The plugin information contains additional properties, including the current plugin state, pointers to the decorated lifecycle methods, and the instantiated plugin class.


## Accessing the Plugin Class

The PluginManager always instantiates the plugin's class that is decorated with `@PluginManager.pluginClass` when the plugin is loaded. By default it is only accessible via Plugin information, but it is not directly exposed for access.

However, it is possible to pass a name for a property in the `@PluginManager.pluginClass` decorator, which will cause the instantiated plugin class to be accessible via a property with that name.

For example, the `HelloWorld` plugin class can be decorated as follows:

```python title="HelloWorld.py with Named Plugin Class"
from acme.runtime import PluginManager

@PluginManager.pluginClass(property='greetings')
class HelloWorld:
    ...
```

The instantiated plugin class can then be accessed via the `PluginManager.greetings` property:

```python title="Getting Named Plugin Class"
hello_world_instance = PluginManager.greetings
```


## Setting the Plugin Class Priority

By default, the `PluginManager` instantiates and processes plugin classes in the order they are loaded. However, one can set a priority for the plugin class instantiation using the `priority` parameter in the `@PluginManager.pluginClass` decorator. This can be useful if a plugin class depends on another plugin class being instantiated first.

Plugin classes with a lower priority value are instantiated before those with a higher priority value. The default priority is `50`.

```python title="HelloWorld.py with Priority"
from acme.runtime import PluginManager

@PluginManager.pluginClass(priority=10)
class HelloWorld:
	...
```

In this example, the `HelloWorld` plugin class will be instantiated before any other plugin classes with the default priority of `50`.

It is important to note that plugin classes with a lower priority value are instantiated first, but are stopped and finalized later, i.e. after plugin classes with a higher priority value, during the CSE shutdown or unloading process. 


## Getting All Plugin Information

The information of all loaded plugins can be accessed via the `PluginManager.plugins` property. This is a list of `PluginInfo` objects, each containing information about a loaded plugin, including its name, module, state, and associated plugin class.

Usually, this is only needed for debugging or introspection purposes. One should be careful when using this property, as it provides access to all loaded plugins and their information, which can lead to unintended consequences if not used properly.

```python title="Getting Plugin Information"
from acme.runtime import PluginManager

# Get a list of all loaded plugins
loaded_plugins = PluginManager.plugins
```

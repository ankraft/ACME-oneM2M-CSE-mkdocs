# Overview

Plugins allow you to extend the functionality of the CSE without modifying its core codebase. 

They are meant to add new features, modify existing behavior, or integrate with external systems. They can fully interact with the CSE's internal APIs and lifecycle events to provide seamless extensions. Plugins have a well-defined lifecycle, including initialization, configuration, activation, deactivation, and cleanup phases.

Another important aspect of plugins is that disabled plugins are not loaded at all, which means that they do not consume any resources and do not have any impact on the CSE's performance. This makes it possible to easily enable or disable features as needed without worrying about their impact on the CSE.

Plugins are standard Python modules that are placed in a *plugins* directory within the CSE's base directory. The CSE automatically loads and integrates these plugins at startup. They are loaded in the Python module namespace `acme.plugins.<package>.<plugin_name>`. Plugins are loaded and processed in alphabetical case-insensitive order based on their filenames.


## Plugin Directories

There are two locations where the CSE looks for plugins during startup:

- The internal *plugins* directory located within the CSE installation path at `acme/plugins`. This directory and its subdirectories are used for plugins that are bundled with the CSE.  
	One should **not** modify or add plugins to this directory, as they may be overwritten during CSE updates.
- The external *plugins* directory located in the CSE's base directory (the directory from which the CSE is started or specified with the [--base-directory <directory>](../setup/Running.md#different-base-directory) command line option). This directory is intended for user-defined plugins.  
	User-defined plugins are loaded under the `plugins` Python module namespace.

Even if these are two different directories, they are treated equally by the CSE. Plugins from both directories are loaded and managed in the same way. The only (runtime) difference is the order in which they are loaded: Plugins from the internal directory are loaded first, followed by plugins from the external directory. This means that plugins in the external directory can override plugins with the same name in the internal directory, but only if this is allowed by the configuration setting described in the next section.


## Replacing Loaded Plugins

All plugins are loaded under the same Python module namespace `plugins` and must therefore all have unique names. If there are plugins with the same name in both directories, a Python Exception will be raised during CSE startup. This behaviour can be changed by setting the configuration option [\[cse.operation.plugins\].replace](../setup/Configuration-cse.md#plugins) to `true`. 


## Disabling User-Provided Plugins

User-Provided Plugins can be disabled by adding them to the configuration option [\[cse.operation.plugins\].disabledPlugins](../setup/Configuration-cse.md#plugins). Plugins listed in this configuration setting will not be loaded by the CSE at startup. 

Names of disabled plugins must match the plugin module name (i.e., the filename without the `.py` extension). Simple wildcard patterns (e.g., `MyPlugin*`) can be used to disable multiple plugins at once.

```ini title="Example Configuration to Disable Plugins"
[cse.operation.plugins]
disabledPlugins = APlugin,MyPlugin*, AnotherPlugin
```

## Dependency Injection for Non-Plugin Classes

The `PluginManager` also provides the ability to inject plugin dependencies into non-plugin classes. This can be useful for classes that depend on the functionality provided by a plugin but are not themselves plugins. By using the [`@PluginManager.requires`](PluginAPI.md#requires) class decorator, one can specify that a class requires a certain plugin, and the `PluginManager` will automatically inject the instance of that plugin into the class when it is instantiated. This allows for seamless integration of plugin functionality into non-plugin classes without having to manually manage dependencies or plugin instances.

# Overview

Plugins allow you to extend the functionality of the CSE without modifying its core codebase. 

They are meant to add new features, modify existing behavior, or integrate with external systems. They can fully interact with the CSE's internal APIs and lifecycle events to provide seamless extensions. Plugins have a well-defined lifecycle, including initialization, configuration, activation, deactivation, and cleanup phases.

Plugins are standard Python modules that are placed in a *plugins* directory within the CSE's base directory. The CSE automatically loads and integrates these plugins at startup. They are loaded in the Python module namespace `acme.plugins.<plugin_name>`. Plugins are loaded and processed in alphabetical case-insensitive order based on their filenames.


## Plugin Directories

There are two locations where the CSE looks for plugins during startup:

- The internal *plugins* directory located within the CSE installation path at `acme/plugins`. This directory is used for plugins that are bundled with the CSE. You should **not** modify or add plugins to this directory, as they may be overwritten during CSE updates.
- The external *plugins* directory located in the CSE's base directory (the directory from which the CSE is started or specified with the [--base-directory <directory>](../setup/Running.md#different-base-directory) command line option). This directory is intended for user-defined plugins. 

Even if these are two different directories, they are treated equally by the CSE. Plugins from both directories are loaded and managed in the same way. The only (runtime) difference is the order in which they are loaded: Plugins from the internal directory are loaded first, followed by plugins from the external directory. This means that plugins in the external directory can override plugins with the same name in the internal directory, but only if this is allowed by the configuration setting described in the next section.


## Replacing Loaded Plugins

All plugins are loaded under the same Python module namespace `acme.plugins` and must therefore all have unique names. If there are plugins with the same name in both directories, a Python Exception will be raised during CSE startup. This behaviour can be changed by setting the configuration option [\[cse.operation.plugins\].replace](../setup/Configuration-cse.md#plugins) to `true`. 


## Disabling Individual Plugins

Plugins can be disabled by adding them to the configuration option [\[cse.operation.plugins\].disabledPlugins](../setup/Configuration-cse.md#plugins). Plugins listed in this configuration setting will not be loaded by the CSE at startup. 

Names of disabled plugins must match the plugin module name (i.e., the filename without the `.py` extension). Simple wildcard patterns (e.g., `MyPlugin*`) can be used to disable multiple plugins at once.

```ini title="Example Configuration to Disable Plugins"
[cse.operation.plugins]
disabledPlugins = APlugin,MyPlugin*, AnotherPlugin
```

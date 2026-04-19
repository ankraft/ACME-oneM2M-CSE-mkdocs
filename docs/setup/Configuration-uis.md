# Configuration - User Interfaces

The CSE provides different user interfaces (UIs) to interact with the CSE. 

## Console

**Section: `[console]`**

These are the settings for the console.

| Setting                     | Description                                                                                                                                                                                              | Default                                                                             |
|:----------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------|
| confirmQuit                 | Quitting the console needs to be confirmed.<br />This may not work under Windows, so it is switched off by default.                                                                                      | False                                                                               |
| headless                    | Run the CSE in headless mode, i.e. without a console and without screen logging.                                                                                                                         | False                                                                               |
| hideResources               | Hide certain resources from display in the console. This is a list of resource identifiers. Wildcards are allowed.                                                                                       | empty list                                                                          |
| refreshInterval             | Interval for continuously refreshing information displays.<br/>Must be > 0.0.                                                                                                                            | 2.0 seconds                                                                         |
| theme                       | Set the color theme for the console.<br /> Allowed values are "dark" and "light".                                                                                                                        | [${basic.config:consoleTheme}](../setup/Configuration-basic.md#basic-configuration) |
| treeIncludeVirtualResources | Show virtual resources in the console's and structure endpoint's tree view.                                                                                                                              | False                                                                               |
| treeMode                    | Set the mode how resources and their content are presented in the console's and structure endpoint's tree view.<br/>Allowed values: `normal`, `compact`, `content`, `contentOnly`                        | normal                                                                              |
| type                        | Set the type of the console.<br />Allowed values are `rich` and `simple`. The `rich` console provides a more advanced user interface, while the `simple` console provides only a minimal user interface. | rich                                                                                | 


## Text UI

**Section: `[textui]`**

These are the settings for the text UI.

| Setting                            | Description                                                                                                                             | Default                      |
|:-----------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| enable                             | Enable the text UI.                                                                                                                     | True                         |
| enableTextEditorSyntaxHighlighting | Enable syntax highlighting in the resource text editor. This setting is only available when the package "textual[syntax]" is installed. | False                        |
| maxRequestSize                     | Max size of a request or response in bytes to display. Requests or responses larger than this threshold will not be displayed.          | 10.000                       |
| notificationTimeout                | Timeout for text UI notifications in seconds.                                                                                           | 2.0                          |
| refreshInterval                    | Interval for refreshing various views in the text UI.                                                                                   | 2.0                          |
| startWithTUI                       | Show the text UI after startup.<br />See also command line argument [–-textui](../setup/Running.md#command-line-arguments).             | False                        |
| theme                              | Set the color theme for the text UI. Allowed values are `dark` and `light`.                                                             | [${console:theme}](#console) |




## Web UI

**Section: `[webui]`**

These are the settings for the web UI.

| Setting | Description              | Default           |
|:--------|:-------------------------|:------------------|
| enable  | Enable the web UI.       | True              |
| root    | Root path of the web UI. | ${http:root}webui |

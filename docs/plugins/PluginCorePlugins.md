# CSE Core Plugins
 
 CSE core plugins are plugins that are used to implement runtime functionalities of the CSE, such as the statistics functionality, the web UI, or the console. They are loaded and managed by the `PluginManager` just like any other plugin, but they are used in a special way by the CSE to provide runtime functionalities.

 When the CSE starts up, it loads all the core plugins for enabled core functionalities from the internal *plugins* directory. This helps to keep the resource usage of the CSE low, as only the internal and third-party Python packages that are actually needed are loaded.

 The following sections lists the currently available core plugins, the functionalities they provid, and the configuration settings that are responsible for enabling them.

## Protocol Bindings

| Plugin Name                           | Functionality                                                            | Responsible Configuration Setting                    |
|:--------------------------------------|:-------------------------------------------------------------------------|:-----------------------------------------------------|
| acme.plugins.bindings.CoAPServer      | Provides the [CoAP protocol](../setup/Configuration-coap.md) binding.    | [\[coap\]:enable](../setup/Configuration-coap.md#)   |
| acme.plugins.bindings.HttpServer      | Provides the [HTTP protocol](../setup/Configuration-http.md) binding.    | [\[http\]:enable](../setup/Configuration-http.md#)   |
| acme.plugins.bindings.MQTTClient      | Provides the [MQTT protocol](../setup/Configuration-mqtt.md) binding.    | [\[mqtt\]:enable](../setup/Configuration-mqtt.md)    |
| acme.plugins.bindings.WebSocketServer | Provides the [WebSocket protocol](../setup/Configuration-ws.md) binding. | [\[websocket\]:enable](../setup/Configuration-ws.md) |


## Console & UIs

| Plugin Name                          | Functionality                                                                                                                                                                                                                                   | Responsible Configuration Setting                                                            |
|:-------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------|
| acme.plugins.bindings.http.HttpWebUI | Provides the [web UI](../setup/WebUI.md) functionality.<br>Runs together with the HTTP server.                                                                                                                                                  | [\[webui\]:enable](../setup/Configuration-uis.md#web-ui)                                     |
| acme.plugins.runtime.Console         | Provides the [console](../setup/Console.md) functionality.<br>Runs together with the CSE core and provides a rich console UI for interacting with the CSE.<br>This is the CSE's default console.                                                | [\[basic.config\]:consoleType](../setup/Configuration-basic.md#basic-configuration) = rich   |
| acme.plugins.runtime.MinimalConsole  | Provides a [minimal console](../setup/Console.md#minimal-console) functionality.<br>Runs together with the CSE core and provides a minimal console UI for interacting with the CSE.<br>This is an alternative, less resource-intensive console. | [\[basic.config\]:consoleType](../setup/Configuration-basic.md#basic-configuration) = simple |


## Runtime

| Plugin Name                     | Functionality                                                                                              | Responsible Configuration Setting                                     |
|:--------------------------------|:-----------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------|
| acme.plugins.runtime.Statistics | Provides the statistics functionality.<br>Runs together with the CSE core and collects runtime statistics. | [\[cse.statistics\]:enable](../setup/Configuration-cse.md#statistics) |



## Management & Testing 

| Plugin Name                                | Functionality                                                                                                                                                               | Responsible Configuration Setting                                                      |
|:-------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------|
| acme.plugins.bindings.http.HttpManagement  | Provides the [HTTP management API](../setup/Operation-management.md) functionality.<br>**This plugin requires the HTTP Server to be enabled.**                                                  | [\[http\]:enableManagementEndpoint](../setup/Configuration-http.md#general-settings)   |
| acme.plugins.bindings.http.HttpStructure   | Provides the [HTTP structure API](../setup/Operation-diagrams.md) functionality.<br>**This plugin requires the HTTP Server to be enabled.**                                                     | [\[http\]:enableStructureEndpoint](../setup/Configuration-http.md#general-settings)    |
| acme.plugins.bindings.http.HttpUpperTester | Provides the [HTTP Upper Tester](../setup/Operation-uppertester.md) functionality.<br>**This plugin requires the HTTP Server to be enabled.**<br/>It is usually only needed for testing purposes.  | [\[http\]:enableUpperTesterEndpoint](../setup/Configuration-http.md#general-settings)  |


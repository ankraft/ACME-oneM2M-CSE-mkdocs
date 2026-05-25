# CSE Core Plugins
 
 CSE core plugins are plugins that are used to implement runtime functionalities of the CSE, such as the statistics functionality, the web UI, or the console. They are loaded and managed by the `PluginManager` just like any other plugin, but they are used in a special way by the CSE to provide runtime functionalities.

 When the CSE starts up, it loads all the core plugins for enabled core functionalities from the internal *plugins* directory. This helps to keep the resource usage of the CSE low, as only the internal and third-party Python packages that are actually needed are loaded.

 The following sections lists the currently available core plugins, the functionalities they provid, and the configuration settings that are responsible for enabling them.

## Protocol Bindings

| Plugin Name                              | Functionality                                                            | Responsible Configuration Setting                    |
|:-----------------------------------------|:-------------------------------------------------------------------------|:-----------------------------------------------------|
| acmecse.plugins.bindings.CoAPServer      | Provides the [CoAP protocol](../setup/Configuration-coap.md) binding.    | [\[coap\]:enable](../setup/Configuration-coap.md#)   |
| acmecse.plugins.bindings.HttpServer      | Provides the [HTTP protocol](../setup/Configuration-http.md) binding.    | [\[http\]:enable](../setup/Configuration-http.md#)   |
| acmecse.plugins.bindings.MQTTClient      | Provides the [MQTT protocol](../setup/Configuration-mqtt.md) binding.    | [\[mqtt\]:enable](../setup/Configuration-mqtt.md)    |
| acmecse.plugins.bindings.WebSocketServer | Provides the [WebSocket protocol](../setup/Configuration-ws.md) binding. | [\[websocket\]:enable](../setup/Configuration-ws.md) |

!!! Note 
	At least one protocol binding plugin must be enabled for the CSE to function properly, as
	the CSE relies on protocol bindings to receive and process requests. 


## Console & UIs

| Plugin Name                             | Functionality                                                                                                                                                                                                                                   | Responsible Configuration Setting                                                            |
|:----------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------|
| acmecse.plugins.bindings.http.HttpWebUI | Provides the [web UI](../setup/WebUI.md) functionality.<br>Runs together with the HTTP server.                                                                                                                                                  | [\[webui\]:enable](../setup/Configuration-uis.md#web-ui)                                     |
| acmecse.plugins.runtime.Console         | Provides the [console](../setup/Console.md) functionality.<br>Runs together with the CSE core and provides a rich console UI for interacting with the CSE.<br>This is the CSE's default console.                                                | [\[basic.config\]:consoleType](../setup/Configuration-basic.md#basic-configuration) = rich   |
| acmecse.plugins.runtime.MinimalConsole  | Provides a [minimal console](../setup/Console.md#minimal-console) functionality.<br>Runs together with the CSE core and provides a minimal console UI for interacting with the CSE.<br>This is an alternative, less resource-intensive console. | [\[basic.config\]:consoleType](../setup/Configuration-basic.md#basic-configuration) = simple |
| acmecse.plugins.runtime.TextUI          | Provides the [text UI](../setup/TextUI.md) functionality.<br>Runs together with the CSE core and provides a text-based user interface for interacting with the CSE.                                                                             | [\[textui\]:enable](../setup/Configuration-uis.md#text-ui)                                   |


## Runtime

| Plugin Name                        | Functionality                                                                                              | Responsible Configuration Setting                                     |
|:-----------------------------------|:-----------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------|
| acmecse.plugins.runtime.Statistics | Provides the statistics functionality.<br>Runs together with the CSE core and collects runtime statistics. | [\[cse.statistics\]:enable](../setup/Configuration-cse.md#statistics) |


## Services

The service plugins implement oneM2M's *Common Service Functions* They run together with the CSE core.


| Plugin Name                                  | Functionality                                                                                                                         | Responsible Configuration Setting                                                         |
|:---------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------|
| acmecse.plugins.services.ActionManager       | Provides the action management services.                                                                                              | [\[cse.service.action\]:enable](../setup/Configuration-cse.md#action-service)             |
| acmecse.plugins.services.AnnouncementManager | Provides the announcement management services.<br>The *acmecse.plugins.services.RemoteCSEManager* plugin is required for this plugin. | [\[cse.service.announcement\]:enable](../setup/Configuration-cse.md#announcement-service) |
| acmecse.plugins.services.GroupManager        | Provides the group management services.                                                                                               | [\[cse.service.group\]:enable](../setup/Configuration-cse.md#group-service)               |
| acmecse.plugins.services.LocationManager     | Provides the location management services.                                                                                            | [\[cse.service.location\]:enable](../setup/Configuration-cse.md#location-service)         |
| acmecse.plugins.services.RemoteCSEManager    | Provides the remote CSE management services.                                                                                          | [\[cse.service.remoteCSE\]:enable](../setup/Configuration-cse.md#remote-cse-service)      |
| acmecse.plugins.services.SemanticManager     | Provides the semantic management services.                                                                                            | [\[cse.service.semantic\]:enable](../setup/Configuration-cse.md#semantic-service)         |
| acmecse.plugins.services.TimeManager         | Provides general time management services.                                                                                            | [\[cse.service.time\]:enable](../setup/Configuration-cse.md#time-service)                 |
| acmecse.plugins.services.TimeSeriesManager   | Provides the timeSeries management services.                                                                                          | [\[cse.service.timeSeries\]:enable](../setup/Configuration-cse.md#timeseries-service)     |

!!! Note
	By disabling a service plugin, the corresponding oneM2M service will not be available in the CSE.
	The related oneM2M resource types may still be available, but they will operate in a limited way.
	Creation or update of resources that rely on the service may cause *NOT_IMPLEMENTED* errors.

	For example, when disabling the *LocationManager* plugin, the &lt;LocationPolicy> resource may 
	still be available, but its functionality will be very limited or even disabled.



## Database

| Plugin Name                                | Functionality                                                                         | Responsible Configuration Setting                                                    |
|:-------------------------------------------|:--------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------|
| acmecse.plugins.database.PostgreSQLBinding | Provides the PostgreSQL database binding functionality.                               | [\[basic.config\]:databaseType](../setup/Configuration-basic.md#basic-configuration) |
| acmecse.plugins.database.TinyDBBinding     | Provides the TinyDB database binding functionality for memory and file-based storage. | [\[basic.config\]:databaseType](../setup/Configuration-basic.md#basic-configuration) |


!!! Note
	Only one database binding is loaded at a time, depending on the value of the *databaseType* setting in
	the basic configuration. 


## Management & Testing 

| Plugin Name                                   | Functionality                                                                                                                                                                                      | Responsible Configuration Setting                                                     |
|:----------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------|
| acmecse.plugins.bindings.http.HttpManagement  | Provides the [HTTP management API](../setup/Operation-management.md) functionality.<br>**This plugin requires the HTTP Server to be enabled.**                                                     | [\[http\]:enableManagementEndpoint](../setup/Configuration-http.md#general-settings)  |
| acmecse.plugins.bindings.http.HttpStructure   | Provides the [HTTP structure API](../setup/Operation-diagrams.md) functionality.<br>**This plugin requires the HTTP Server to be enabled.**                                                        | [\[http\]:enableStructureEndpoint](../setup/Configuration-http.md#general-settings)   |
| acmecse.plugins.bindings.http.HttpUpperTester | Provides the [HTTP Upper Tester](../setup/Operation-uppertester.md) functionality.<br>**This plugin requires the HTTP Server to be enabled.**	<br/>It is usually only needed for testing purposes. | [\[http\]:enableUpperTesterEndpoint](../setup/Configuration-http.md#general-settings) |


!!! Note
	The HTTP management API, structure API and upper tester functionalities are only available
	if the HTTP Server plugin is enabled, as they rely on the HTTP protocol to provide their functionalities.	
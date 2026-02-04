# Configuration - CSE Registration

The CSE settings are used to configure the CSE's registration behaviour towards a Registrar CSE,
allowed originators for AE and CSR registrations, and resource announcement behaviour.


## CSE Registration 

**Section: `[cse.registration]`**

These settings are used to configure the CSE's internal registration behaviour, but also set the allowed originators for AE and CSR registrations.

| Setting                | Description                                                                                                                                                                                                   | Default    |
|:-----------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------|
| allowedAEOriginators   | List of AE originators that can register. This is a comma-separated list of originators. Wildcards (\* and ?) are supported.                                                                                  | C\*, S\*   |
| allowedCSROriginators  | List of CSR originators that can register. This is a comma-separated list of originators. Wildcards (\* and ?) are supported.<br />**Note**: CSE-IDs must be in absolute or SP-relative form.                 | empty list |
| checkLiveliness        | Check the liveliness of the registrations to the registrar CSE and also from the registree CSEs.                                                                                                              | True       |
| checkInterval          | This setting specifies the pause in seconds between tries to connect to the configured registrar CSE. This value is also used to check the connectivity to the registrar CSE after a successful registration. | 60 seconds |
| unregisterWhenStopping | Unregister from the registrar CSE when stopping the CSE. This includes deleting the CSR resource at the registrar CSE.                                                                                        | True       |


## Registrar CSE Access 

**Section: `[cse.registrar]`**

These settings are used to configure the address, access and general behavior to a Registrar CSE.

| Setting              | Description                                                                                                                 | Default                                                                                                                         |
|:---------------------|:----------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------|
| address              | URL of the Registrar CSE.                                                                                                   | [http://${basic.config:registrarCseHost}:${basic.config:registrarCsePort}](../setup/Configuration-basic.md#basic-configuration) |
| root                 | Registrar CSE root path. Never provide a trailing `/`.                                                                      | empty string                                                                                                                    |
| cseID                | CSE-ID of the Registrar CSE.<br />A CSE-ID must start with a `/`, and must not contain a further `/` or white space.        | /[${basic.config:registrarCseID}](../setup/Configuration-basic.md#basic-configuration)                                          |
| resourceName         | The Registrar CSE's resource name.                                                                                          | [${basic.config:registrarCseName}](../setup/Configuration-basic.md#basic-configuration)                                         |
| INCSEcseID           | The CSE-ID of the Infrastructure CSE at the top of the deployment tree.                                                     | /id-in                                                                                                                          |
| serialization        | Specify the serialization type that must be used for the registration to the registrar CSE.<br />Allowed values: json, cbor | json                                                                                                                            |
| excludeCSRAttributes | Comma separated list of attributes that are excluded when creating a registrar CSR.                                         | empty list                                                                                                                      |
| originator           | The originator used for the registration to the registrar CSE.                                                              | The CSE's CSE ID                                                                                                                |


## Registrar CSE Security Settings

**Section: `[cse.registrar.security]`**

| Setting          | Description                                                                                                                                         | Default      |
|:-----------------|:----------------------------------------------------------------------------------------------------------------------------------------------------|:-------------|
| httpUsername     | The username used for the Registrar CSE authentication via *http* if basic authentication is enabled for the Registrar CSE.                         | empty string |
| httpPassword     | The password used for the Registrar CSE authentication via *http* if basic authentication is enabled for the Registrar CSE.                         | empty string |
| httpBearerToken  | The authentication token used for the Registrar CSE authentication via *http* if bearer token authentication is enabled for the Registrar CSE.      | empty string |
| wsUsername       | The username used for the Registrar CSE authentication via *WebSocket* if basic authentication is enabled for the Registrar CSE.                    | empty string |
| wsPassword       | The password used for the Registrar CSE authentication via *WebSocket* if basic authentication is enabled for the Registrar CSE.                    | empty string |
| wsBearerToken    | The authentication token used for the Registrar CSE authentication via *WebSocket* if bearer token authentication is enabled for the Registrar CSE. | empty string |
| selfHttpUsername | The own CSE's username used for the Registrar CSE authentication via *http* if basic authentication is enabled for the Registrar CSE.               | empty string |
| selfHttpPassword | The own CSE's password used for the Registrar CSE authentication via *http* if basic authentication is enabled for the Registrar CSE.               | empty string |
| selfWsUsername   | The own CSE's username used for the Registrar CSE authentication via *WebSocket* if basic authentication is enabled for the Registrar CSE.          | empty string |
| selfWsPassword   | The own CSE's password used for the Registrar CSE authentication via *WebSocket* if basic authentication is enabled for the Registrar CSE.          | empty string |


## Service Provider Registrations

**Section: `[cse.sp.registrar.{SP Name}]`**

These settings are used to configure the behavior when registering to another service provider's Infrastructure CSE. 
If one wants to register to more than one service provider, one must create a separate section for each service provider 
with a name or identifier for the service provider as part of the section name.

The section name has a fixed prefix of `cse.sp.registrar.` followed by a unique identifier for that particular service provider registration. This identifier is also used in the optional security settings section for that service provider registration.

| Setting              | Description                                                                                                                                                   | Default          |
|:---------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------|
| spID                 | The oneM2M Service Provider ID (SP-ID) of the service provider to regiser to.<br/>It must start with `//`, and must not contain a further `/` or white space. | none             |
| address              | URL of the other service provider's IN-CSE.                                                                                                                   | none             |
| root                 | The service provider's IN-CSE root path. Never provide a trailing `/`.                                                                                        | empty string     |
| cseID                | CSE-ID of the service provider's IN-CSE.<br/>A CSE-ID must start with a `/`, and must not contain a further `/` or white space.                               | no default       |
| excludeCSRAttributes | Comma separated list of attributes that are excluded when creating a registrar CSR.                                                                           | empty list       |
| resourceName         | The service provider's IN-CSE's resource name.                                                                                                                | no default       |
| INCSEcseID           | The CSE-ID of the Infrastructure CSE at the top of the deployment tree.                                                                                       | /id-in           |
| serialization        | Specify the serialization type that must be used for the registration to the service provider's IN-CSE.<br />Allowed values: json, cbor                       | json             |
| originator           | The originator used for the registration to the service provider's IN-CSE.                                                                                    | The CSE's CSE ID |


## Service Provider Security Settings

**Section: `[cse.sp.registrar.security.{SP Name}]`**

These settings are used to configure the security settings when registering to another service provider's Infrastructure CSE. 
If one wants to register to more than one service provider, one must create a separate section for each service provider
with a name or identifier for the service provider as part of the section name.

The section name must match the section name of the service provider's registration settings, i.e.

```ini title="Example of a service provider's registration section, including security settings"
[cse.sp.registrar.MySP]
...

[cse.sp.registrar.security.MySP]
...	
```


| Setting          | Description                                                                                                                                                                 | Default      |
|:-----------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------|
| httpUsername     | The username used for the service provider's IN-CSE authentication via *http* if basic authentication is enabled for the service provider's IN-CSE.                         | empty string |
| httpPassword     | The password used for the service provider's IN-CSE authentication via *http* if basic authentication is enabled for the service provider's IN-CSE.                         | empty string |
| httpBearerToken  | The authentication token used for the service provider's IN-CSE authentication via *http* if bearer token authentication is enabled for the service provider's IN-CSE.      | empty string |
| wsUsername       | The username used for the service provider's IN-CSE authentication via *WebSocket* if basic authentication is enabled for the Registrar CSE.                                | empty string |
| wsPassword       | The password used for the service provider's IN-CSE authentication via *WebSocket* if basic authentication is enabled for the Registrar CSE.                                | empty string |
| wsBearerToken    | The authentication token used for the service provider's IN-CSE authentication via *WebSocket* if bearer token authentication is enabled for the service provider's IN-CSE. | empty string |
| selfHttpUsername | The own CSE's username used for the service provider's IN-CSE authentication via *http* if basic authentication is enabled for the service provider's IN-CSE.               | empty string |
| selfHttpPassword | The own CSE's password used for the service provider's IN-CSE authentication via *http* if basic authentication is enabled for the service provider's IN-CSE.               | empty string |
| selfWsUsername   | The own CSE's username used for the service provider's IN-CSE authentication via *WebSocket* if basic authentication is enabled for the service provider's IN-CSE.          | empty string |
| selfWsPassword   | The own CSE's password used for the service provider's IN-CSE authentication via *WebSocket* if basic authentication is enabled for the service provider's IN-CSE.          | empty string |


## Resource Announcements

**Section: `[cse.announcements]`**

These settings are used to configure the behavior of resource announcements. They control mainly internal CSE behaviour and are not directly related to the oneM2M standard.

| Setting                        | Description                                                                                                                 | Default    |
|:-------------------------------|:----------------------------------------------------------------------------------------------------------------------------|:-----------|
| checkInterval                  | Wait n seconds between tries to announce resources to registered remote CSE.                                                | 10 seconds |
| allowAnnouncementsToHostingCSE | Allow resource announcements to the own hosting CSE.                                                                        | True       |
| delayAfterRegistration         | Specify a short delay in seconds before starting announcing resources after a remote CSE has registered at the hosting CSE. | 3 seconds  |


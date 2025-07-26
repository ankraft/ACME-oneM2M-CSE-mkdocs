# Configuration - HTTP Binding Settings

The CSE supports HTTP binding for communication with clients and other CSEs. The HTTP binding is always enabled and its settings are configured in the configuration file under the section `[http]` and its subsections.

##	General Settings

**Section: `[http]`**

These are the general settings for the HTTP binding.

| Setting                   | Description                                                                                                                                                                                                                                                                                                                                                                                                                        | Default                                                                                                                                                               |
|:--------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| port                      | Port to listen to.                                                                                                                                                                                                                                                                                                                                                                                                                 | [${basic.config:httpPort}](../setup/Configuration-basic.md#basic-configuration)                                                                                       |
| listenIF                  | Interface to listen to. Use 0.0.0.0 for "all" interfaces.                                                                                                                                                                                                                                                                                                                                                                          | [${basic.config:networkInterface}](../setup/Configuration-basic.md#basic-configuration)                                                                               |
| address                   | Own address. Should be a local/public reachable address.                                                                                                                                                                                                                                                                                                                                                                           | http://[${basic.config:cseHost}](../setup/Configuration-basic.md#basic-configuration):[${basic.config:httpPort}](../setup/Configuration-basic.md#basic-configuration) |
| root                      | CSE Server root. Never provide a trailing `/`.                                                                                                                                                                                                                                                                                                                                                                                     | empty string                                                                                                                                                          |
| enableManagementEndpoint  | Enable the management interface for the CSE. This allows to manage the CSE and retrieve information about its operation via HTTP requests.<br />**ATTENTION: Enabling this feature may expose sensitive information. It may also lead to a total loss of data.**<br/>See also [CSE Management](../setup/Operation-management.md).                                                                                                  | False                                                                                                                                                                 |
| enableStructureEndpoint   | Enable an endpoint for getting a structured overview about a CSE's resource tree and deployment infrastructure (remote CSE's).<br />**ATTENTION: Enabling this feature exposes various potentially sensitive information.**<br/>See also the [*\[console\].hideResources*](../setup/Configuration-uis.md#console) setting to hide resources from the tree.<br/>See also [Infrastructure Diagrams](../setup/Operation-diagrams.md). | False                                                                                                                                                                 |
| enableUpperTesterEndpoint | Enable an endpoint for supporting Upper Tester commands to the CSE. This is to support certain testing and certification systems. See oneM2M's TS-0019 for further details.<br/>**ATTENTION: Enabling this feature may lead to a total loss of data.**<br/>See also [Upper Tester Support](../setup/Operation-uppertester.md) for more information.                                                                                | False                                                                                                                                                                 |
| allowPatchForDelete       | Allow the http PATCH method to be used as a replacement for the DELETE method. This is useful for constraint devices that only support http/1.0, which doesn't specify the DELETE method.<br/>**ATTENTION: This is an enhanced feature of the ACME CSE, and not part of the oneM2M HTTP protocol binding specification.**                                                                                                          | False                                                                                                                                                                 |
| timeout                   | Timeout when sending http requests and waiting for responses.                                                                                                                                                                                                                                                                                                                                                                      | 10 seconds                                                                                                                                                            |


## Security

**Section: `[http.security]`**

These are the security settings for the HTTP binding.

!!! see-also "See also"
	[HTTP Authentication](../setup/Certificates.md#http-authentication)


| Setting           | Description                                                                                                                                                                                                                                                                                                 | Default                                                                                                        |
|:------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------|
| useTLS            | Enable TLS for communications.<br />This can be overridden by the command line arguments [--http and --https](Running.md).<br />See oneM2M TS-0003 Clause 8.2.1 "Overview on Security Association Establishment Frameworks".                                                                                | False                                                                                                          |
| tlsVersion        | TLS version to be used in connections. <br />Allowed versions: `TLS1.1`, `TLS1.2`, `auto` . Use `auto` to allow client-server certificate version negotiation.                                                                                                                                              | auto                                                                                                           |
| verifyCertificate | Verify certificates in requests. Set to *False* when using self-signed certificates.                                                                                                                                                                                                                        | False                                                                                                          |
| caCertificateFile | Path and filename of the certificate file.                                                                                                                                                                                                                                                                  | empty string                                                                                                   |
| caPrivateKeyFile  | Path and filename of the private key file.                                                                                                                                                                                                                                                                  | empty string                                                                                                   |
| enableBasicAuth   | Enable basic authentication for the HTTP binding. See [HTTP Basic Authentication](../setup/Certificates.md#basic-authentication).                                                                                                                                                                           | False                                                                                                          |
| enableTokenAuth   | Enable token authentication for the HTTP binding. See [HTTP Token Authentication](../setup/Certificates.md#bearer-token-authentication).                                                                                                                                                                    | False                                                                                                          |
| basicAuthFile     | Path and filename of the http basic authentication file.<br/>The file must contain lines in the format "username:hashed-password". Lines starting with a # character are treated as comments. The passwords are stored in hashed form (see [Hashing Credentials](../development/tools/HashCredentials.md)). | [${basic.config:baseDirectory}](../setup/Configuration-basic.md#basic-configuration)/certs/http_basic_auth.txt |
| tokenAuthFile     | Path and filename of the http bearer token authentication file.<br/>The file must contain lines in the format "hashed-token". Lines starting with a # character are treated as comments. The tokens are stored in hashed form (see [Hashing Credentials](../development/tools/HashCredentials.md))          | [${basic.config:baseDirectory}](../setup/Configuration-basic.md#basic-configuration)/certs/http_token_auth.txt |



## CORS

**Section: `[http.cors]`**

These are the CORS (Cross-Origin Resource Sharing) settings for the HTTP binding.

| Setting   | Description                                                                                             | Default                                                |
|:----------|:--------------------------------------------------------------------------------------------------------|:-------------------------------------------------------|
| enable    | Enable CORS support for the HTTP binding.                                                               | False                                                  |
| resources | A comma separated list of allowed resource paths. The list elements could be regular expressions.<br /> | "/*" , ie. all resources under the HTTP server's root. |


## WSGI 

**Section: `[http.wsgi]`**

These are the settings for the WSGI (Web Server Gateway Interface) support.

| Setting         | Description                                                                                                                                | Default |
|:----------------|:-------------------------------------------------------------------------------------------------------------------------------------------|:--------|
| enable          | Enable WSGI support for the HTTP binding.                                                                                                  | False   |
| threadPoolSize  | The number of threads used to process requests. This number should be of similar size as the *connectionLimit* setting.                    | 100     |
| connectionLimit | The number of possible parallel connections that can be accepted by the WSGI server. Note: One connection uses one system file descriptor. | 100     |

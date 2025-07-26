# Configuration - CoAP Binding Settings

The CSE supports CoAP communication via the CoAP binding. The CoAP binding is disabled by default and must be enabled in the configuration file under `[coap].enable` .

##	General Settings

**Section: `[coap]`**

These are the general CoAP client settings.

| Setting                   | Description                                                                                                                                                                        | Default                                                                                       |
|:--------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------|
| enable                    | Enable the CoAP binding.                                                                                                                                                           | False                                                                                         |
| port                      | Set the listening port for the CoAP server.                                                                                                                                        | 5683                                                                                          |
| listenIF                  | Interface to listen to. Use 0.0.0.0 for "all" interfaces.                                                                                                                          | [${basic.config:networkInterface}](../setup/Configuration-basic.md#basic-configuration)       |
| timeout                   | Timeout when sending CoAP requests and waiting for responses.                                                                                                                      | 10.0 seconds                                                                                  |
| clientConnectionCacheSize | The maximum number of client connections that can be cached. When the limit is reached, the oldest connection is closed and removed from the cache. A value of 0 means no caching. | 100                                                                                           |
| address                   | Own address. Should be a local/public reachable address.                                                                                                                           | coap://[${basic.config:cseHost}](../setup/Configuration-basic.md#basic-configuration):${port} |


## Security

**Section: `[coap.security]`**

These are the security settings for the CoAP binding.

!!! warnig "Attention"
	Security for the CoAP binding is currently not implemented. The settings are reserved for future use.

| Setting           | Description                                                                                                                                                    | Default      |
|:------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------|
| useTLS            | Enable TLS for CoAP communications.                                                                                                                            | False        |
| dtlsVersion       | TLS version to be used in connections. <br />Allowed versions: `TLS1.1`, `TLS1.2`, `auto` . Use `auto` to allow client-server certificate version negotiation. | auto         |
| verifyCertificate | Verify certificates in requests. Set to False when using self-signed certificates.                                                                             | False        |
| caCertificateFile | Path and filename of the certificate file.                                                                                                                     | empty string |
| caPrivateKeyFile  | Path and filename of the private key file.                                                                                                                     | empty string |


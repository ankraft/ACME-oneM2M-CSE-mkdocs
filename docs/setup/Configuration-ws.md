# Configuration - WebSocket Binding Settings

The CSE supports WebSocket communication via the WebSocket binding. The WebSocket binding is disabled by default and must be enabled in the configuration file under `[websocket].enable`.

## General Settings

**Section: `[websocket]`**

These are the general WebSocket settings.

| Setting  | Description                                                                                    | Default                                                                                     |
|:---------|:-----------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------|
| enable   | Enable the WebSocket binding.                                                                  | False                                                                                       |
| port     | Set the port for the WebSocket server.                                                         | 8180                                                                                        |
| listenIF | Interface to listen to. Use 0.0.0.0 for "all" interfaces.                                      | 0.0.0.0                                                                                     |
| address  | Own address. Should be a local/public reachable address.                                       | ws://[${basic.config:cseHost}](../setup/Configuration-basic.md#basic-configuration):${port} |
| loglevel | Loglevel for the WebSocket server. Allowed values: `debug`, `info`, `warning`, `error`, `off`. | [${basic.config:logLevel}](../setup/Configuration-logging.md)                               |
| timeout  | Timeout when sending websocket requests and waiting for responses.                             | [${cse:requestExpirationDelta}](../setup/Configuration-cse.md#general-settings)       |


## Security

**Section: `[websocket.security]`**

These are the security settings for the WebSocket binding.

!!! see-also "See also"
	[WebSocket Authentication](../setup/Certificates.md#websocket-authentication)



| Setting           | Description                                                                                                                                                                                                                                                                                                      | Default                                                                                                      |
|:------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------|
| useTLS            | Enable TLS for websocket communications.                                                                                                                                                                                                                                                                         | False                                                                                                        |
| tlsVersion        | TLS version to be used in connections. <br />Allowed versions: `TLS1.1`, `TLS1.2`, `auto` . Use `auto` to allow client-server certificate version negotiation.                                                                                                                                                   | auto                                                                                                         |
| verifyCertificate | Verify certificates in requests. Set to False when using self-signed certificates.                                                                                                                                                                                                                               | False                                                                                                        |
| caCertificateFile | Path and filename of the certificate file.                                                                                                                                                                                                                                                                       | empty string                                                                                                 |
| caPrivateKeyFile  | Path and filename of the private key file.                                                                                                                                                                                                                                                                       | empty string                                                                                                 |
| enableBasicAuth   | Enable basic authentication for the WebSocket binding.                                                                                                                                                                                                                                                           | False                                                                                                        |
| enableTokenAuth   | Enable token authentication for the WebSocket binding.                                                                                                                                                                                                                                                           | False                                                                                                        |
| basicAuthFile     | Path and filename of the WebSocket basic authentication file.<br/>The file must contain lines in the format "username:hashed-password". Lines starting with a # character are treated as comments. The passwords are stored in hashed form (see [Hashing Credentials](../development/tools/HashCredentials.md)). | [${basic.config:baseDirectory}](../setup/Configuration-basic.md#basic-configuration)/certs/ws_basic_auth.txt |
| tokenAuthFile     | Path and filename of the WebSocket bearer token authentication file.<br/>The file must contain lines in the format "hashed-token". Lines starting with a # character are treated as comments. The tokens are stored in hashed form (see [Hashing Credentials](../development/tools/HashCredentials.md))          | [${basic.config:baseDirectory}](../setup/Configuration-basic.md#basic-configuration)/certs/ws_token_auth.txt |

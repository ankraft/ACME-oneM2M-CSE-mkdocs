# Configuration - MQTT Binding Settings

The CSE supports MQTT communication via the MQTT binding. The MQTT binding is disabled by default and must be enabled in the configuration file under `[mqtt].enable` .

##	General Settings

**Section: `[mqtt]`**

These are the general MQTT client settings.

| Setting     | Description                                                      | Default               |
|:------------|:-----------------------------------------------------------------|:----------------------|
| enable      | Enable the MQTT binding.                                         | False                 |
| address     | The hostname of the MQTT broker.                                 | 127.0.0.1             |
| port        | Set the port for the MQTT broker.                                | 1883, or 8883 for TLS |
| listenIF    | Interface to listen to. Use 0.0.0.0 for "all" interfaces.        | 0.0.0.0               |
| keepalive   | Value for the MQTT connection's keep-alive parameter in seconds. | 60 seconds            |
| topicPrefix | Optional prefix for topics.                                      | empty string          |
| timeout     | Timeout when sending MQTT requests and waiting for responses.    | 10.0 seconds          |


## Security

**Section: `[mqtt.security]`**

These are the security settings for the MQTT binding.

| Setting              | Description                                                                                                                                                                                            | Default      |
|:---------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------|
| useTLS               | Enable TLS for communications with the MQTT broker.                                                                                                                                                    | False        |
| username             | The username for MQTT broker authentication if required by the broker.                                                                                                                                 | empty string |
| password             | The password for MQTT broker authentication.                                                                                                                                                           | empty string |
| verifyCertificate    | Verify certificates in requests. Set to False when using self-signed certificates.                                                                                                                     | False        |
| caCertificateFile    | Path and filename of the certificate file.                                                                                                                                                             | empty string |
| allowedCredentialIDs | List of credential-IDs that can be used to register an AE via MQTT. If this list is empty then all credential IDs are allowed.<br />This is a comma-separated list. Wildcards (* and ?) are supported. | empty list   |


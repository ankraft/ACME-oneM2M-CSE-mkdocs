# Certificates and Hashes

The ACME CSE supports the secure protocol versions of HTTP, MQTT, and WebSockets. This means that you can use the CSE with HTTPS, MQTT over TLS, and WSS.

To enable, for example, https you have to set various settings under the security configuration [http.security](../setup/Configuration-http.md#security), and provide a certificate and a key file. The other protocols are configured in a similar way.

Please note, that TLS is only supported for the non-WSGI-based HTTP server, so if you are using the WSGI-based server, you have to use a reverse proxy (e.g. nginx) to enable TLS.

!!! see-also "See also"
	[HTTP Security Settings](../setup/Configuration-http.md#security)  
	[MQTT Security Settings](../setup/Configuration-mqtt.md#security)  
	[WebSocket Security Settings](../setup/Configuration-ws.md#security)

## Location of Certificates and Authentication Files

The certificates and authentication files used by the ACME CSE are typically stored in a dedicated directory under the runtime base directory. By default, this directory is named *cert*. You can change this location and the filenames by setting the appropriate configuration parameters in the *acme.ini* configuration file.

## Self-Signed Certificates

One way to create those files is the [openssl](https://www.openssl.org){target=_new} tool that may already be installed on your OS. The following example shows how to create a self-signed certificate:

```bash title="Create a self-signed certificate"
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -nodes -days 1000
```

This will create the self-signed certificate and private key without password protection (*-nodes*), and stores them in the files *cert.pem* and *key.pem* respectively. *openssl* will prompt you with questions for *Country Name* etc, but you can just hit *Enter* and accept the defaults. The *-days* parameter affects the certificate's expiration date.

Please also consult the *openssl* manual for further instructions. 

After you generated these files you can move them to a separate directory (for example you may create a new directory named *cert* under ACME's [runtime base directory](../setup/Running.md#different-base-directory)) and set the *caCertificateFile* and *caPrivateKeyFile* configuration parameters in the *acme.ini* configuration file under the appropriate security section(s) accordingly.

## HTTP Authentication

The ACME CSE supports the following HTTP authentication methods.

!!! warning
	Note that the credentials are partly stored as plain text (ie. user names) and in hashed form (ie. passwords and tokens) in the respective credential files. Make sure to protect these files accordingly.

### Basic Authentication

The ACME CSE supports basic authentication. To enable it, you have to set the [[http.security].enableBasicAuth](../setup/Configuration-http.md#security) configuration parameter in the *acme.ini* configuration file to *True* .

As a default, the path and filename for the basic authentication file is *certs/http_basic_auth.txt*.  
This file must be in the format *username:password*, and comments (lines starting with #) are allowed.

The *password* is **not** stored as plain text but as a hashed password. 

You may set the [[http.security].basicAuthFile](../setup/Configuration-http.md#security) configuration parameter to a different path or filename user credentials if you do not want to use the default location.

#### Example: Basic Authentication File

```text
# This is a comment
user1:hashedPassword1
user2:hashedPassword2
```

#### Create a Hashed Password

To create a hashed password for basic authentication, you can use the [hashcreds](../development/tools/HashCredentials.md) tool.


#### Usage in HTTP Request

To access the CSE, you have to provide the credentials in the HTTP request header in the form of ```Authorization: Basic <credentials>```, where &lt;credentials> is the Base64 encoding of a username and (unhashed) password joined by a single colon.



=== "curl HTTP Request using Authorization header"
	```bash
	curl -H "Authorization: Basic dXNlcjE6cGFzc3dvcmQx" http://example.com
	```
=== "curl HTTP Request using -u option"

	```bash
	curl -u user:hashedPassword http://example.com
	```


### Bearer Token Authentication

The ACME CSE also supports a simple form of bearer token authentication. To enable it, you have to set the [[http.security].enableTokenAuth](../setup/Configuration-http.md#security) configuration parameter in the *acme.ini* configuration file to *True* .

As a default, the path and filename for the token authentication file is *certs/http_token_auth.txt*.  
The file must contain lines with each token on a separate line. Comments (lines starting with #) are allowed.  

The token is **not** stored as plain text but as a hashed token. 

You may set the [[http.security].tokenAuthFile](../setup/Configuration-http.md#security) configuration parameter to a different path or filename for the bearer token file if you do not want to use the default location.


#### Example: Bearer Token Authentication File

```text
# This is a comment
hashedToken1
hashedToken2
```

#### Create a Hashed Token

To create a hashed token for the bearer token authentication file, you can use the [hashcreds](../development/tools/HashCredentials.md) tool.

#### Usage in HTTP Request

In this case to access the CSE, you have to provide the (unhashed) bearer token in the HTTP request header in the form of ```Authorization: Bearer <token>```.


```sh title="curl HTTP Request using Authorization header"
curl -H "Authorization: Bearer hashedToken" http://example.com
```



## WebSocket Authentication

The ACME CSE supports the same authentication methods for the WebSocket binding as for the HTTP binding. To enable basic or token authentication for the WebSocket binding, you have to set the [[websocket.security].enableBasicAuth](../setup/Configuration-ws.md#security) or [[websocket.security].enableTokenAuth](../setup/Configuration-ws.md#security) configuration parameter in the *acme.ini* configuration file to *True* .

The configuration parameters are the same as for the HTTP binding. See [HTTP Authentication](#http-authentication) for further details on how to configure the authentication files.

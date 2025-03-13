# How to Enable Authentication Between CSEs

For the *Mca* communication between an AE and a CSE, the CSE supports [Basic Authentication](../setup/Configuration-http.md#security) and [Bearer Token Authentication](../setup/Configuration-http.md#security). These methods are supported for the *HTTP* and *WebSocket* bindings. The same authentication methods can be used for the communication between CSEs. 


## Authenticating to the Registrar CSE
For example, to enable *basic authentication* to authenticate access to the registrar CSE, one has to set the currect *username* and *password* in the *acme.ini* configuration file of the registrar CSE in the [\[cse.registrar.security\]](../setup/Configuration-cse.md#registrar-cse-security) section, for example

```ini title="Setting the username and password for http <i>basic authentication</i> with the registrar CSE"
[cse.registrar.security]
httpUsername=aUsername
httpPassword=aPassword
```

The CSE then uses these credentials to authenticate the *Mcc* communication with the registrar CSE. Of course, the registrar CSE must be configured to accept these credentials. The same applies to the *WebSocket* binding. A similar configuration can be made for *Bearer Token Authentication* for the *http* or *WebSocket* bindings.

## Authenticating Requests from the Registrar CSE

In return it is also possible to provide credentials that the registrar CSE must use for the *Mcc* communication with this CSE. *http* and *WebSocket* requests from the registrar CSE to this registree CSE can be authenticated using the same methods as described above.

The configuration for the CSE is done as well in the [\[cse.registrar.security\]](../setup/Configuration-cse.md#registrar-cse-security) section of the *acme.ini* configuration file:


```ini title="Setting the username and password for http <i>basic authentication</i> to be used by the registrar CSE"
[cse.registrar.security]
selfHttpUsername=aUsername
selfHttpPassword=aPassword
```

Currently, only the *http* and *WebSocket* bindings support this kind of authentication, and only the *basic* authentication method is supported.

!!! Note
	This method adds the credentials to the URL in the *point of access* (PoA) attribute of the remote *CSR* resource.  
	This is not a secure method and should only be used in a secure and trusted environment.  
	Also, the implementation of the registrar CSE must support this method of authentication.


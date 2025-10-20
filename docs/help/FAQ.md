# FAQ

## Installation and Running

1. **Can I install the ACME CSE on a Raspberry Pi?**  
   Yes, the ACME CSE can be installed and run on a Raspberry Pi. However, this usually requires to install a newer version of Python than the one that is installed by default on the Raspberry Pi, and to install some additional libraries, tools, and packages.  
   See the HowTo [Install ACME on a Raspberry Pi](../howtos/RaspberryPi.md){target=_new} for further details.
## Network

1. **How can I access the CSE from remote/another computer on my network?**  
   By default the CSE binds to the *localhost/loopback* interface, meaning it **will not** be able to receive requests from remote machines. To make it accessible from a remote machine you need to bind the CSE's http server or MQTT client to another network interface, or address. This can be done in the *[coap]*, *[http]*, *[mqtt]* and *[websocket]* sections of the configuration file. 
   Setting the listen interface to "0.0.0.0" binds the servers to all available interfaces.  
   The reason for this default setting is security: making the CSE accessible from remote machines should be a conscious decision and not the default.


## Database

1. **Corrupt database files**  
   In very rare cases, e.g. when the CSE was not properly shut down, the on-disk database files for TinyDB may be corrupted. The CSE tries to detect this during start-up, but there is not much one can do about this. However, a backup of the database file is created every time the CSE starts. This backup can be found in the *backup* sub-directory of the *data* directory. 


## HTTP Binding

1. **What does the error message "[Errno 13] Permission denied" during startup of the CSE mean?**  
   This error is shown by the CSE when the http server tries to bind to a TCP/IP port to listen for incoming requests, 
   but doesn't have enough privileges to do so. This usually happens when an http port < 1024 is configured (e.g. 80) and 
   the CSE is run with normal user privileges. Either run the CSE with admin / superuser rights (NOT recommended), 
   or choose another TCP/IP port, larger than 1024.
1. **Can the ACME CSE be accessed when the installation requires a URL path prefix?**  
	The ACME CSE does support URL path prefixes. For example, if the CSE is configured to listen on port 8080 and the path prefix is ```/acme```, then the CSE and its resources can be accessed via the URL `http://hostname:8080/acme/&lt;oneM2M resource path>`.  
	To set the path prefix, use the configuration setting *[http].root* in the *acme.ini* file:

	```ini title="Example to set the HTTP URL path prefix to '/acme'"
	[http]
	root=/acme
	```


1. **Is there a work-around for the missing DELETE method in http/1.0?**  
   Many constraint devices only support version 1.0 of the http protocol. This version of http, though, does not specify the
   DELETE method, which means that those devices cannot invoke oneM2M's DELETE operation.  
   The ACME CSE implements an experimental work-round by supporting the http PATCH operation in addition to the normal DELETE
   operation: Instead of sending oneM2M DELETE requests using the http DELETE method one can send the same request with the http PATCH method.  
   This feature is disabled by default and can be enabled by setting the configuration setting *[server.http].allowPatchForDelete*
   to *true*.
1. **Is there a way to enable CORS (Cross-Origin Resource Sharing) for ACME?**  
   CORS allows browser-based applications to access resources on a web server outside the domain of the
   original hosting web server. This could be useful, for example, to allow a web UI that is hosted on 
   one web server to access oneM2M resources that are hosted on external CSE(s).  
   ACME's http binding implementation supports CORS. This feature is disabled by default and can be 
   enabled by setting the configuration setting *[http.cors].enable* to *true*. CORS access is granted
   by default to all HTTP resources. This can be limited by specifying the resource paths in the 
   configuration setting *[http.cors].resources*.  
   **Note**: Most modern web browsers don't allow unsecured (http) access via CORS. This means that the
   CSE must be configured to run the http server with TLS support enabled (https).


## MQTT Binding

1. **What does the error message "Out of memory" mean that appears sometimes?**  
   This error message should actually read "connection refused" or "general error" that is returned by the underlying MQTT library. The error code "1" indicates this error but the human readable error message seems to be wrongly assigned here.
1. **What is going on when the error "rc=7: The connection was lost" is repeatedly thrown?**  
   This error message might occur when another client (perhaps another running CSE with the same CSE-ID) connected to an MQTT broker with the same client ID. The CSE then tries to re-connect and the other CSE is disconnected by the broker. And then this client tries to reconnect. This will then repeat over and over again.  
   Identify the other client, stop it, and assign it a different CSE-ID.
1. **What does "cannot connect to broker: [Errno 49] Can't assign requested address" mean?**  
   You most likely want to connect to an MQTT broker that does not run on your local machine and you configured the listen interface to "127.0.0.1", which means that only local running services can be reached. Try to set the configuration *[client.mqtt].listenIF* to "0.0.0.0".


## Resources

1. **How can I add my own FlexContainer specializations to the ACME CSE?**  
   All resources and specializations are validated by the CSE. You can add your own specializations and validation policies by providing them in one or more separate files in the *import* directory. Those files must have the file extension ".ap". These files are read during the startup of the CSE.
   See [the documentation about defining FlexContainer spezializations](../development/FlexContainerPolicies.md) for further details.


## CSE Registrations

1. **Why are there regular checks for remote CSEs?**  
	When a CSE is configured as an MN-CSE or ASN-CSE it can register to a remote CSE, respectively an IN-CSE and MN-CSE can receive connection requests from those CSE types. A &lt;remoteCSE> resource is created in case of a successful registration. A CSE checks regularly the connection to other remote CSEs and removes the *remoteCSE* if the connection could not been established. This is done to keep the CSE's resource tree clean and up-to-date.  
	This behaviour can be disabled by setting the configuration setting [`[cse.registration].checkLiveliness`](../setup/Configuration-cse.md#cse-registration) to *false*.  
	The check interval can be configured with the [`[cse.registration].checkInterval`](../setup/Configuration-cse.md#cse-registration) setting.


1. **Why does my CSE not register to another CSE or announce resources?**  
   One problem could be that the CSE has no access rights to register to the target CSE. To solve this, the CSE's originator (ie. the CSE's CSE-ID, for example "/id-mn") must be added to the target CSE's configuration file. The configuration section [cse.registration] has a setting *allowedCSROriginators*, which is a comma separated list of originators. Add the registering CSE's
   CSE-ID in ^^absolute^^{title="e.g. //MySP/id-in"} or ^^SP-relative^^{title="e.g. /id-in"} format to this configuration section to allow access for this originator.  
   <br/>
   This must be done for both the CSEs that want to register and announce resources.  
   </br>
   Example for an IN-CSE with the CSE-ID "*/id-in*":

	```ini title="Example to allow a CSE with the CSE-ID 'id-mn' to register to the IN-CSE"
	[cse.registration]
	allowedCSROriginators=/id-mn
	```
   <br/>
   And for an MN-CSE with the CSE-ID "*/id-mn*":

	```ini title="Example to allow the IN-CSE with the CSE-ID 'id-in' to get access"
	[cse.registration]
	allowedCSROriginators=/id-in
	```

1. **The CSE is able to register to another CSE, but the registration vanishes after a short time. What is happening?**  
  This could happen when the registering CSE is not reachable. During the registration process the normal configuration settings are used to establish the registration. This includes the protocol, server address and port, etc.  
  But afterwards, the *point-of-access* (*poa*) attribute of the *&lt;remoteCSE>* (*CSR*) resource is used to determine the protocol and endpoint to reach the remote CSE. If this value is incorrect and/or the checking CSE cannot reach the remote CSE, then the checking CSE removes the registration for that remote CSE. This might happen, for example, when the CSE is behind a NAT or running in a Docker container, and the *poa* contains only the private or local IP address of the CSE.  
  <br/>
  During startup, the CSE tries to determine its public IP address and set the *poa* attribute(s) accordingly. But this process might fail for various reasons:  
  	- **The CSE is behind a NAT in a private network and cannot determine its public IP address**.  
	In this case, the best way is to set the *cseHost* setting in the configuration section *[basic.config]* manually to the correct public IP address.  
	Also, do not forget to add a proper port forwarding rule
	to your router's configuration.
 	- **The CSE is running in a Docker container and the *poa* contains the container's private IP address**.  
	This is a variant of the previous case. One way to solve this problem is to use an environment variable to pass, for example, the Docker host's public IP address. See the [discussion about Environment Variables](../howtos/Docker.md#environment-variables) in a Docker runtime environment.
  	- **The CSE cannot determine its IP address for other reasons**.  
	If for some other reason the CSE cannot determine its IP address, one of the previous solutions can be applied.
	- **One or both CSE is behind a firewall**.  
	In this case, the firewall might block incoming or outgoing connections from or to the checking CSE, leading to the removal of the registration. A proper firewall configuration or port forwarding to the remote CSE is necessary to allow these connections.

## Subscriptions & Notifications

1. **Why does creating a &lt;subscription> resource sometimes throw an error?**  
   When an AE creates a &lt;subscription> resource a CSE may send a *verification notification* to the configured *notificationURI* address(es) to verify that these endpoints exist and that they can receive notifications. If the verification request fails, for example if there is (yet) no server that can receive the notifications, the whole CREATE request of the &lt;subscription> resource fails.  
   By default, this *verification notification* procedure is enabled in ACME, but it can be disabled by setting the following [configuration](../setup/Configuration-cse.md#general-settings) in the *acme.ini* file:
   ```ini title="Disable verification notification requests"
   	[cse]
	enableSubscriptionVerificationRequests=false
   ```
   With this, the CSE will not verify the notification endpoints and the &lt;subscription> resource creation will succeed (if there are no other problems, of course).
   
## Improving Performance

1. **How to improve the performance of ACME CSE?**  
   The log output provides useful information to analyze the flows of requests inside the CSE. However, it reduces the performance of the CSE by a lot. Reducing the log level to *info* or *warning* already helps. This can be done in the *[logging]* section of the configuration file, or by pressing `shift-L` on the console to change the logging level to the desired value. Also, [disabling writing the log to a file](../setup/Configuration-logging.md) will increase the performance.  
   Another option is to change the database to *memory* mode. This means that all database access happens in memory and not on disk. But please be aware that this also means that all data will be lost when the CSE terminates!  
   Lastly, the ACME CSE can be run with newer versions of Python, which is way faster and more efficient than previous versions of Python.

1. **Improve database performance with *disk* mode**  
   When running the CSE with the database mode set to *tinydb* (ie. store the database on disk rather then in memory) one can improve the performance by [increasing the time before data is actually written to disk](../setup/Configuration-database.md#tinydb). The default is 1 second, but it can be increased as necessary.  
   Be aware, though, that the risk of losing data increases with higher delays in case of a crash or when the CSE shutdown is interrupted.

	```ini title="Example to set the write delay to 10 seconds"
	[database.tinydb]
	writeDelay=10
	```

1. **Reduce various check-intervals**  
   The CSE checks various things at regular intervals, such as the liveliness of remote CSEs, expired resources, 
   and so on. The following table list various intervals that can be configured to reduce the load on the CSE and the network.
   The default values are usually sufficient for development use cases, but can be reduced to a lower value for deployment use cases.
   
	| Setting                             | Description                                                                                                                                                                                   |
	|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
	| \[cse].checkExpirationsInterval     | The interval in seconds to check for expired resources. This can be set to a much higher value than the default of 60 seconds if it is not necessary to expire resources quickly.             |
	| \[cse.registration].checkInterval   | The interval in seconds to check the liveliness of remote CSEs. This behaviour can even be disabled by setting the following configuration option to `false`.                                 |
	| \[cse.registration].checkLiveliness | Whether to check the liveliness of remote CSEs. This can be disabled to reduce the load on the CSE and the network. Set this to `false` to disable the check.                                 |
	| \[cse.statistics].writeInterval     | The interval in seconds to write the statistics to the database. This can be set to a much higher value than the default of 60 seconds if it is not necessary to write statistics frequently. |
	| \[scripting].fileMonitoringInterval | The interval in seconds to check for changes in the scripting files. This can be set to a higher value if it is not necessary to check for changes frequently.                                |

1. **Disable request recording**  
   if the configuration setting *[cse.operation.requests].enable* is set to *true*, then CSE records all requests and responses. This is useful for debugging and testing, but it can also slow down the CSE significantly.
   To disable this feature, set the configuration setting to *false* in the *acme.ini* configuration file:

	```ini title="Disable request recording"
	[cse.operation.requests]
	enable=false
	```

## Web UI

1. **Can I use the web UI also with other CSE implementations?**  
    The web UI can also be run as an independent application.  Since it communicates with the CSE via the Mca interface it should be possible to use it with other CSE implementations as well as long as those third party CSEs follow the oneM2M http binding specification. It only supports the resource types that the ACME CSE supports, but at least it will present all other resource types as *unknown*.


## Console and Text UI

1. **Some of the tables, text graphics etc are not aligned or correctly displayed in the console**  
	Some mono-spaced fonts don't work well with UTF-8 character sets and graphic elements. Especially the MS Windows *cmd.exe* console seems to have problems.
	Try one of the more extended fonts like *JuliaMono* or *DejaVu Sans Mono*.
1. **There is an error message "UnicodeEncodeError: 'latin-1' codec can't encode character"**  
	This error message is shown when the console tries to display a character that is not supported by the current console encoding. Try to set the console encoding to UTF-8 by setting the environment variable *PYTHONIOENCODING* to *utf-8*, for example:

	```bash title="Set the terminal console encoding to UTF-8"
	export PYTHONIOENCODING=utf-8
	``` 
	
## Operating Systems

### RaspberryPi

1. **Restrictions on 32 bit Systems**  

	!!! Note
		This answer is a bit outdated. Newer Raspberry Pi models are 64 bit systems and can run the 64 bit version of Raspberry Pi OS.  
		Still, the following information might be useful for older models or for other 32 bit systems.

	Currently, the normally installed Raspbian OS is a 32 bit system. This means that several restrictions apply here, such as the maximum date supported (~2038). It needs to be determined whether these restrictions still apply when the 64 bit version of Raspbian is available.
1. **The console or the text UI is not displayed correctly**  
	It could be that the OS's terminal applications doesn't support rendering of extra characters, like line graphics. One recommendation on Linux systems is to install the [Mate Terminal](https://wiki.mate-desktop.org/mate-desktop/applications/mate-terminal/){target=_new}, which supports UTF-8 and line graphics. It also renders the output much faster.

	```bash title="Install the Mate Terminal"
	sudo apt-get install mate-terminal
	```

1. **Timing Issues**  
	 Also, the resolution of the available Python timers is rather low on Raspbian 32 Bits, and background tasks might not run exactly on the desired time.  
	 Unfortunately, this is also why sometimes a couple of the CSE's tests cases may fail randomly.

# CSE Runtime Features

In addition to the oneM2M standard functionalities, 
the ACME CSE implements additional features to help with deployments and to
understand and learn about oneM2M.


## Database Bindings

The following database bindings are supported:

**PostgreSQL**

:	PostgreSQL is a powerful, open-source object-relational database system. 
	The ACME CSE can be configured to use a local or remote PostgreSQL database for data storage.


**TinyDB "file-based"**

:	[TinyDB](https://github.com/msiemens/tinydb){target=_new} is a fast and lightweight 
	document oriented database that is ideal for small installations.


**TinyDB "in-memory"**

:	[TinyDB](https://github.com/msiemens/tinydb){target=_new} can also be used as an in-memory database. 
	This is useful for testing and small installations.

	This database binding offers the fastest processing speed and response times.
	However, it offers no persistence of data between restarts.


## Additional CSE Runtime Features

The ACME CSE provides the following additional features:


**Guided Setup**

:	An interactive setup process that guides the user through the initial configuration of the CSE.


**HTTP Authorization**

:	Basic support for *basic* and *bearer* (token) authorization.


**HTTP CORS**

:	Support for *Cross-Origin Resource Sharing* to support http(s) redirects.

	*[CORS]: Cross-Origin Resource Sharing


**HTTP WSGI**

:	Support for the Python *Web Server Gateway Interface* to improve integration with a reverse proxy or API gateway, ie. Nginx.

	*[WSGI]: Web Server Gateway Interface


**Recording Requests**

:	Requests over Mca and Mcc to an from a CSE can be recorded. This may be used to inspect
	communication sequences between oneM2M entities and to debug requests.


**Remote Configuration**

:	Besides using a local configuration file, the ACME CSE can also use a [Apache Zookeeper](https://zookeeper.apache.org/){target=_new} server to [retrieve its configuration](../setup/Configuration-introduction.md#using-apache-zookeeper-for-configuration).


**Script Interpreter**

:	The CSE includes a [Lisp-based script interpreter](../development/ACMEScript.md) to extent CSE functionalities, 
	implement simple AEs, prototypes, tests, and more.


**Testing: Upper Tester**

:	Basic support for the [Upper Tester](../setup/Operation-uppertester.md) protocol defined in oneM2M's [TS-0019](https://specifications.onem2m.org/ts-0019){target=_new},
	and additional command execution support.


**Text Console**

:	Control and manage the CSE, inspect resources, and run scripts in a text [console](../setup/Console.md).  
	The log output is also displayed in the text console.


**Text UI**

:	A [text-based user interface](../setup/TextUI.md) to inspect resources and requests, configurations, 
	stats, manage resources, display diagrams, and more.


**Web UI**

:	A [Web UI](../setup/WebUI.md) that displays the oneM2M resource tree and offers a basic REST UI.


## Runtime Environments

The ACME CSE runs at least on the following runtime environments:

- Generic Linux  
	Including [Raspberry Pi OS (32bit)](../howtos/RaspberryPi.md) on [Raspberry Pi](../howtos/RaspberryPi.md).
- macOS
- MS Windows
- Jupyter Notebooks  
	ACME CSE can be run headless inside a [Jupyter Notebook](../development/Embedding_ACME.md#jupyter-notebooks).
- Docker  
	ACME CSE can be run in a [Docker container](../howtos/Docker.md).


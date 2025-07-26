# Configuration - Introduction

The CSE is highly configurable and can be adapted to different environments and requirements. This article describes the configuration settings and how to change them.

## Introduction

Configuration of CSE parameters is primarily done through a configuration file. This file contains all configurable and customizable
settings for the CSE. Configurations are mostly optional, and settings in this file overwrite the CSE's default values.

The configuration file follows the Windows INI file format with sections, setting and values. A configuration file may include comments, prefixed with the characters `#` or `;` .

### Command Line Arguments

Also, some settings can be applied via the [command line](../setup/Running.md#command-line-arguments) when starting the CSE. These command line arguments overwrite the
settings in the configuration file.

## The *acme.ini* Configuration File

!!! warning
	Changes should only be done to a copy of the default configuration file.

A default configuration is provided with the file [acme.ini.default](https://github.com/ankraft/ACME-oneM2M-CSE/blob/master/acme/init/acme.ini.default){target=_new}. The settings in this file are the default values for the CSE and can be overwritten by local configuration file.  
 **Don't make changes to the default configuration file**, but rather copy relevant configuration setting to a new file named, for example, *acme.ini*, which is the default configuration file name. You can use another filename, but must then specify it with the `--config` command line argument when running the (see [Running the CSE](../setup/Running.md#running-the-cse)).

It is sufficient to only add the settings to the configuration file that are different from the default settings. All other settings are read from the default config file *acme.ini.default*.

<figure markdown="1">
![Figure 1: Steps when reading a configuration from the <i>acme.ini</i> file](../images/acme.ini.png#only-light){data-gallery="light"}
![Figure 1: Steps when reading a configuration from the <i>acme.ini</i> file](../images/acme.ini-dark.png#only-dark){data-gallery="dark"}
<figcaption>Figure 1: Steps when reading a configuration from the <i>acme.ini</i> file</figcaption>
</figure>


If the configuration file *acme.ini* could not be found at the specified location then an interactive procedure is started to generate a file with basic configuration settings. You can add further configurations if necessary by copying sections and settings from *acme.ini.default*.

!!!	info
	It is highly recommended to use this interactive procedure to create the configuration file. This ensures that all necessary settings are present and that the file is correctly formatted.


## Using Apache Zookeeper for Configuration

The CSE can also be configured using [Apache Zookeeper](https://zookeeper.apache.org/){target=_new}. This allows for a more dynamic configuration and is especially useful in distributed environments. The configuration settings are stored in Zookeeper and can be accessed by the CSE at runtime.

<figure markdown="1">
![Figure 2: Steps when reading a configuration from Apache Zookeeper](../images/zookeeper.ini.png#only-light){data-gallery="light"}
![Figure 2: Steps when reading a configuration from Apache Zookeeper](../images/zookeeper.ini-dark.png#only-dark){data-gallery="dark"}
<figcaption>Figure 2: Steps when reading a configuration from Apache Zookeeper</figcaption>
</figure>


In this case, a local configuration file (e.g. *acme.ini*) is **not** used, and the CSE is started with the command line arguments [--config-zk-host](Running.md#command-line-arguments) and [--config-zk-root](Running.md#command-line-arguments) to specify the Zookeeper server and the root configuration node. The CSE will then read the configuration settings from Zookeeper. 

!!! info
	Similar to using the *acme.ini* configuration file, it is sufficient to only add the settings to the Zookeeper configuration that are different from the default settings. All other settings are read from the default config file *acme.ini.default*.

One can create a Zookeeper-based configuration using the [Zookeeper Tool](../development/tools/ZookeeperTool.md). First, create a configuration file using either the [onboarding tool](../development/tools/OnboardingTool.md) or do it manually. Then, use the Zookeeper Tool to upload the configuration to Zookeeper. The [Zookeeper Tool](../development/tools/ZookeeperTool.md) will create the necessary nodes and set the configuration settings in Zookeeper.

!!! info
	Zookeeper-based configurations are stored in a hierarchical structure. The root node of this structure must be unique for each CSE and is specified with the `--config-zk-root` command line argument. It is recommended to use the CSE-ID as the root node name, e.g. `/cse/cseID`.

### Example: Starting the CSE with Zookeeper Configuration

```bash title="Starting the CSE with Zookeeper configuration"
acmecse --config-zk-host localhost:2181 --config-zk-root id-in
```


## Settings Interpolation

In addition to assigning individual or fixed values for configurations settings you can use [settings interpolation](https://docs.python.org/3/library/configparser.html#interpolation-of-values){target=_new} which allows you to reference settings from the same or from other sections. The syntax to denote a value from a section is ```${section:option}```.


### Built-in Settings

There are some built-in configuration settings that can be used in the configuration file. These settings are provided by the CSE and can be used to reference directories or other values.


**${basic.config:baseDirectory}**  
**${baseDirectory}**
:	Two built-in configuration settings that point to the base-directory of the CSE's data directory. These settings contain  either the current working directory or the directory that is specified with the command line argument `--base-directory` or `-dir`.  
	Both settings are equivalent and can be used interchangeably.


**${configfile}**
:	Configuration setting that contains the name of the configuration file in the *baseDirectory*.


**${hostIPAddress}**
:	Built-in configuration setting that contains the current IP address of the CSE host.


**${basic.config:initDirectory}**  
**${initDirectory}**
:	Two built-in configuration settings that point to acme's main *init* directory.  
	Both settings are equivalent and can be used interchangeably.

	```ini title="Use built-in settings"
	[cse]
	resourcesPath=${basic.config:initDirectory}
	```

**${basic.config:moduleDirectory}**  
**${moduleDirectory}**
:	Two built-in configuration settings that point to acme's module directory.
	Both settings are equivalent and can be used interchangeably.


**${secret}**
:	Built-in configuration setting that contains the main secret key for the CSE. This key is used, for example, to seed passwords for hashing.  
	The default for	this setting is `acme`, and it is highly recommended to change it to a unique value.  
	It is also possible to use the environment variable [ACME_SECURITY_SECRET](#environment-variables)
	to set this value.

### Environment Variables

You can also use environment variables in the configuration file. The syntax to get their values is also `${ENVIRONMEMNT_VARIABLE_NAME}`.

Environment variables can be used in the configuration file to provide sensitive information like passwords or API keys, or to provide a more flexible way to set configuration settings.


#### Hiding Sensitive Information

Sensitive information like passwords or API keys should not be stored in the configuration file in plain text. Instead, you can use environment variables to store this information and reference them in the configuration file.

The following environment variables are supported by default for configurations and don't need to be defined separately in the configuration file:

| Environment Variable                         | Description                                                                                                                                                                                                                 | Configuration Setting                                                                       |
|----------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| ACME_SECURITY_SECRET                         | The main secret key for the CSE. This key is used, for example, so seed passwords for hashing.                                                                                                                              | [\[cse.security\].secret](./Configuration-cse.md#general-security)                          |
| ACME_DATABASE_POSTGRESQL_PASSWORD            | Password to authenticate with the PostgreSQL database                                                                                                                                                                       | [\[database.postgresql\].password](./Configuration-database.md#postgresql)                  |
| ACME_MQTT_SECURITY_PASSWORD                  | Password to authenticate with the MQTT broker                                                                                                                                                                               | [\[mqtt.security\].password](./Configuration-mqtt.md#security)                              |
| ACME_MQTT_SECURITY_USERNAME                  | Username to authenticate with the MQTT broker                                                                                                                                                                               | [\[mqtt.security\].username](./Configuration-mqtt.md#security)                              |
| ACME_CSE_REGISTRAR_SECURITY_HTTPUSERNAME     | Username for HTTP Basic Authentication with the registrar CSE                                                                                                                                                               | [\[cse.registrar.security\].httpUsername](./Configuration-cse.md#registrar-cse-security)    |
| ACME_CSE_REGISTRAR_SECURITY_HTTPPASSWORD     | Password for HTTP Basic Authentication with the registrar CSE                                                                                                                                                               | [\[cse.registrar.security\].httpPassword](./Configuration-cse.md#registrar-cse-security)    |
| ACME_CSE_REGISTRAR_SECURITY_HTTPBEARERTOKEN  | Bearer token for HTTP Bearer Token Authentication with the registrar CSE                                                                                                                                                    | [\[cse.registrar.security\].httpBearerToken](./Configuration-cse.md#registrar-cse-security) |
| ACME_CSE_REGISTRAR_SECURITY_WSUSERNAME       | Username for WebSocket Basic Authentication with the registrar CSE                                                                                                                                                          | [\[cse.registrar.security\].wsUsername](./Configuration-cse.md#registrar-cse-security)      |
| ACME_CSE_REGISTRAR_SECURITY_WSPASSWORD       | Password for WebSocket Basic Authentication with the registrar CSE                                                                                                                                                          | [\[cse.registrar.security\].wsPassword](./Configuration-cse.md#registrar-cse-security)      |
| ACME_CSE_REGISTRAR_SECURITY_WSBEARERTOKEN    | Bearer token for WebSocket Bearer Token Authentication with the registrar CSE                                                                                                                                               | [\[cse.registrar.security\].wsBearerToken](./Configuration-cse.md#registrar-cse-security)   |
| ACME_CSE_REGISTRAR_SECURITY_SELFHTTPUSERNAME | The CSE's own wsername for HTTP Basic Authentication with the CSE by a registrar CSE.<br>See also [Authentication Between CSEs](../howtos/AuthenticationBetweenCSEs.md#authenticating-requests-from-the-registrar-cse)      | [\[cse.security\].httpUsername](./Configuration-cse.md#registrar-cse-security)              |
| ACME_CSE_REGISTRAR_SECURITY_SELFHTTPPASSWORD | The CSE's own password for HTTP Basic Authentication with the CSE by a registrar CSE.<br>See also [Authentication Between CSEs](../howtos/AuthenticationBetweenCSEs.md#authenticating-requests-from-the-registrar-cse)      | [\[cse.security\].httpPassword](./Configuration-cse.md#registrar-cse-security)              |
| ACME_CSE_REGISTRAR_SECURITY_SELFWSUSERNAME   | The CSE's own username for WebSocket Basic Authentication with the CSE by a registrar CSE.<br>See also [Authentication Between CSEs](../howtos/AuthenticationBetweenCSEs.md#authenticating-requests-from-the-registrar-cse) | [\[cse.security\].wsUsername](./Configuration-cse.md#registrar-cse-security)                |
| ACME_CSE_REGISTRAR_SECURITY_SELFWSPASSWORD   | The CSE's own password for WebSocket Basic Authentication with the CSE by a registrar CSE.<br>See also [Authentication Between CSEs](../howtos/AuthenticationBetweenCSEs.md#authenticating-requests-from-the-registrar-cse) | [\[cse.security\].wsPassword](./Configuration-cse.md#registrar-cse-security)                |



#### Docker Host IP

Another useful application is to provide the IP address of a Docker host to the CSE. This can be done, for example, by setting the environment variable `DOCKER_HOST_IP` and using it in the configuration file.

```ini title="Use Environment Variable to set the Host IP"
[basic.config]
cseHost=${DOCKER_HOST_IP}
```

## Dot Notation

Configuration settings can be accessed and updated from scripts, for example with the [get-config](../development/ACMEScript-functions.md#get-config) function. For this, the section and setting names are concatenated using dot notation. The full section name is followed by a dot and then the setting name.

For example:

- The *cseID* setting in the *[cse]* section may be accessed by the name `cse.cseID`
- The *host* setting in the *[database.postgresql]* section may be accessed by the name `database.postgresql.host`

# Onboarding Tool

The ACME CSE can be configured by an interactive onboarding process when it is started for the first time. This process will create a configuration file that can be edited later.

## Running

It is also possible to run the onboarding process at any time without starting the CSE. This can be useful if the configuration file was deleted or if the configuration needs to be changed.

The onboarding process is started by running the onboarding tool from the command line. The tool will guide you through the configuration process and save the configuration to a file.

=== "For Package Installation "

	Run the following command from the command line of your terminal program from **within any directory that uses the Python environment where you installed the package**:

	```bash title="Running the onboarding tool"
	acmecse-onboarding acme.ini
	```

=== "For Manual Installation"

	Run the following command from the command line from **within the directory where you installed the CSE**:

	```bash title="Running the onboarding tool as a module"
	python3 -m acme.onboarding acme.ini
	```


This will start the configuration process and save the configuration to the specified file. The configuration file can be edited later.

Beside the basic configuration options, the onboarding tool also supports to set advanced configuration options. This is optional and can be skipped.


## Command Line Arguments

The onboarding tool provides the following command line arguments.


| Command Line Argument       | Description                                            |
|:----------------------------|:-------------------------------------------------------|
| -h, --help                  | Show a help message and exit.                          |
| --overwrite, -o             | Overwrite the configuration file if it already exists. |
| --zookeeper-host <hostname> | Specify the Zookeeper host name.                       |
| --zookeeper-port <port>     | Specify the Zookeeper port (default: 2181).            |

The name of the configuration file is the only required argument.
If the configuration file already exists, the tool will terminate unless the `--overwrite` argument is provided.


## Zookeeper Support

The onboarding tool can be run with Zookeeper support by specifying the `--zookeeper-host` and optional `--zookeeper-port` command line arguments. This allows the tool to connect to a Zookeeper instance.

During the onboarding process, the tool will ask for some Zookeeper-specific configuration options, such as the Zookeeper host, port, and root path. 

!!! Note
    The root path is the Zookeeper node where the CSE's configuration is stored. If left empty, the CSE-ID will be used as the root path.

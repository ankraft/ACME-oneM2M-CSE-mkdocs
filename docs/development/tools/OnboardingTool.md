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


## Command Line Arguments

The onboarding tool provides the following command line arguments.


| Command Line Argument | Description                                            |
|:----------------------|:-------------------------------------------------------|
| -h, --help            | Show a help message and exit.                          |
| --overwrite, -o       | Overwrite the configuration file if it already exists. |

The name of the configuration file is the only required argument.
If the configuration file already exists, the tool will terminate unless the `--overwrite` argument is provided.
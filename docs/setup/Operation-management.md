# Operation - CSE Management API

If enabled, the CSE provides a management API that allows to retrieve configuration and status information, stream logs and requests
as well as to reset, shutdown and restart the CSE. This API is intended for operational tasks and monitoring of the CSE.

Management commands can be sent to the CSE via HTTP requests.


## Enabling CSE Management

The CSE management API is disabled by default. To enable it, set the [*enableManagementEndpoint*](../setup/Configuration-http.md#general-settings) setting in the configuration file to `true`:

```ini title="Enable CSE Management"
[http]
enableManagementEndpoint = true
```

## Management Commands

The CSE management interface provides several commands that can be used to manage the CSE and retrieve information about its operation. The commands are sent as HTTP requests to the management endpoint, which is  located at `/__mgmt__` of the CSE's HTTP server.

| Command                                                                             | Description                                                                                                                                                                                                     |
|:------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| help                                                                                | Show a list of available management commands.                                                                                                                                                                   |
| creds<br>creds/reload<br>                                                           | ==development feature==<br>Managing credentials. For example, reloading the credentials for HTTP and WebSocket authentication.                                                                                  |
| config                                                                              | Get the current configuration of the CSE in JSON format.                                                                                                                                                        |
| log                                                                                 | Stream the live log output of the CSE. The log will continue to stream until the connection is closed.                                                                                                          |
| loglevel<br>loglevel/&lt;level>                                                     | Get or set the log level of the CSE. The log level can be set to `info`, `debug`, `warn`, `error`, or `off`.                                                                                                    |
| registrations<br>registrations/refresh                                              | Get the current registrations of the CSE in JSON format. This includes the registrations to remote CSEs, service providers and the registrations of local AEs.<br>Also, initiate a manual registration refresh. |
| requests<br>requests/enable<br>requests/disable<br>requests/status<br>requests/puml | Stream a live output of the current requests of the CSE in JSON format as well as enable, disable and get the status of request recording.                                                                      |
| reset                                                                               | Reset the CSE to its initial state. This will clear all resources from the CSE.                                                                                                                                 |
| restart                                                                             | Shutdown the CSE to restart it. The CSE will **not** restart internally, but it will exit with an exit code 82. See also the example below.                                                                     |
| shutdown                                                                            | Shutdown the CSE normally. The CSE will exit with an exit code 0.                                                                                                                                               |
| status<br>status/modules<br>status/plugins<br>status/services.                      | Get the current status of the CSE in JSON format. This includes information about the CSE resources, operational parameters, plugins, requests, and services.                                                   |

All commands with subcommands (e.g., `status` and `requests`) also have a *help* subcommand that provides more detailed information about the available subcommands and their usage. For example, to get more information about the `status` command, you can send the following request:

```bash title="Get help for status command"
curl -X GET http://localhost:8080/__mgmt__/status/help
```

## Examples

The following examples show how to send requests to the management API using [curl](https://curl.se){target="_blank"}.

### Get the CSE's Configuration

```bash title="Get CSE Configuration"
curl -X GET http://localhost:8080/__mgmt__/config
```

This will return the current configuration of the CSE in JSON format. 

### CSE Log Streaming

```bash title="Stream CSE Logs"
curl -X GET http://localhost:8080/__mgmt__/log
```
This will stream the live log output of the CSE.  The log will continue to stream until the connection is closed, e.g. by pressing `Ctrl+C` in the terminal.


### CSE Logging

```bash title="CSE Logging"
# Get the CSE's Log Level
curl -X GET http://localhost:8080/__mgmt__/loglevel

# Set the CSE's Log Level, e.g. to debug
curl -X GET http://localhost:8080/__mgmt__/loglevel/debug

#Disable CSE Log Output"
curl -X GET http://localhost:8080/__mgmt__/loglevel/off
```


### Shutdown the CSE

```bash title="Shutdown CSE"
curl -X GET http://localhost:8080/__mgmt__/shutdown
```


### Restart the CSE

```bash title="Restart CSE"
curl -X GET http://localhost:8080/__mgmt__/restart
```
This will shutdown the CSE and exit with an exit code 82. 

The CSE will **not** restart automatically. To support this one needs
to check the exit code of the CSE and restart it manually or via a script.

The following example code shows how to restart the CSE automatically using a shell script:

=== "Bash Shell"
	```bash title="Restart CSE with Bash Shell"
	while true; do
		python -m acme
		if [ $? -ne 82 ]; then
			break
		fi
		echo "Restarting ACME CSE..."
	done
	```

=== "Fish Shell"
	```fish title="Restart CSE with Fish Shell"
	while true
		python -m acme
		if test $status -ne 82
			break
		end
		echo "Restarting ACME CSE..."
    end
	```

### Stream CSE Requests

```bash title="Stream CSE Requests"
# Stream CSE Requests
curl -X GET http://localhost:8080/__mgmt__/requests
```

### Request Recording

```bash title="Request Recording"

# Enable Request Recording
curl -X GET http://localhost:8080/__mgmt__/requests/enable

# Disable Request Recording
curl -X GET http://localhost:8080/__mgmt__/requests/disable

# Get Request Recording Status
curl -X GET http://localhost:8080/__mgmt__/requests/recording/status
```

### CSE Registrations

```bash title="CSE Registrations"
# Get CSE Registrations
curl -X GET http://localhost:8080/__mgmt__/registrations

# Refresh CSE Registrations
curl -X GET http://localhost:8080/__mgmt__/registrations/refresh
```

### CSE Status

```bash title="CSE Status"
# Get CSE Status
curl -X GET http://localhost:8080/__mgmt__/status

# Get Loaded Python Modules
curl -X GET http://localhost:8080/__mgmt__/status/modules

# Get Loaded Plugins, their Status and Dependencies
curl -X GET http://localhost:8080/__mgmt__/status/plugins

# Get Registered Services
curl -X GET http://localhost:8080/__mgmt__/status/services
```
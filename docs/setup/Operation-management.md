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

| Command       | Description                                                                                                                                                    |
|:--------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| help          | Show a list of available management commands.                                                                                                                  |
| config        | Get the current configuration of the CSE in JSON format.                                                                                                       |
| log           | Stream the live log output of the CSE. The log will continue to stream until the connection is closed.                                                         |
| loglevel      | Get or set the log level of the CSE. The log level can be set to `info`, `debug`, `warn`, `error`, or `off`.                                                   |
| registrations | Get the current registrations of the CSE in JSON format. This includes the registrations to remote CSEs, service providers and the registrations of local AEs. |
| requests      | Stream a live output of the current requests of the CSE in JSON format as well as enable, disable and get the status of request recording.                     |
| reset         | Reset the CSE to its initial state. This will clear all resources from the CSE.                                                                                |
| restart       | Shutdown the CSE to restart it. The CSE will **not** restart internally, but it will exit with an exit code 82. See also the example below.                    |
| shutdown      | Shutdown the CSE normally. The CSE will exit with an exit code 0.                                                                                              |
| status        | Get the current status of the CSE in JSON format. This includes information about the CSE resources, operational parameters and requests.                      |


## Examples

The following examples show how to use the management commands via `curl`.

### Get the CSE's Configuration

```bash title="Get CSE Configuration"
curl -X GET http://localhost:8080/__mgmt__/config
```

This will return the current configuration of the CSE in JSON format. 

### Get the CSE's Log Output

```bash title="Get CSE Log Output"
curl -X GET http://localhost:8080/__mgmt__/log
```

This will stream the live log output of the CSE. The log will continue to stream until the connection is closed, e.g. by pressing `Ctrl+C` in the terminal.


### Get the CSE's Log Level

```bash title="Get CSE Log Level"
curl -X GET http://localhost:8080/__mgmt__/loglevel
```


### Set the CSE's Log Level

```bash title="Set CSE Log Level to Debug"
curl -X GET http://localhost:8080/__mgmt__/loglevel/debug
```

```bash title="Disable CSE Log Output"
curl -X GET http://localhost:8080/__mgmt__/logloglevel/off
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
	done
	```

=== "Fish Shell"
	```fish title="Restart CSE with Fish Shell"
	while true
        python -m acme
        if test $status -ne 82
            break
        end
    end
	```


### Stream CSE Requests

```bash title="Stream CSE Requests"
curl -X GET http://localhost:8080/__mgmt__/requests
```


### Enable Request Recording

```bash title="Enable Request Recording"
curl -X GET http://localhost:8080/__mgmt__/requests/enable
```


### Disable Request Recording
```bash title="Disable Request Recording"
curl -X GET http://localhost:8080/__mgmt__/requests/disable
```


### Get the Request Recording Status
```bash title="Get Request Recording Status"
curl -X GET http://localhost:8080/__mgmt__/requests/recording/status
```

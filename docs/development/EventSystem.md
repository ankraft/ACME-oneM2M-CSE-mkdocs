# Event System

The ACME CSE includes an event system that allows synchronous and asynchronous event handling.
It is used internally by the CSE to handle various runtime events, such as resource creation, updates, deletions, registrations, and more. It can also be used by plugins to define their own event handlers for internal CSE events or for custom events defined by the plugins themselves.


## List of Events

The following events are available by default in the ACME CSE and are used internally by the CSE to trigger various actions and behaviors. These events can also be used by user-provided plugins to hook into the CSE's internal processes and implement custom behavior.

### CSE Related Events

=== "Lifecycle"
	| Event Name   | Payload | Sync / Async | Description                                     |
	|:-------------|:--------|:-------------|:------------------------------------------------|
	| cseStartup   | None    | Async        | Triggered when the CSE finishes startup.        |
	| cseShutdown  | None    | Sync         | Triggered when the CSE is shutdown.             |
	| cseReset     | None    | Sync         | Triggered when the CSE is reset.                |
	| cseRestarted | None    | Sync         | Triggered after the CSE has finished the reset. |

=== "Configuration"
	| Event Name   | Payload      | Sync / Async | Description                                                               |
	|:-------------|:-------------|:-------------|:--------------------------------------------------------------------------|
	| configUpdate | Key<br>Value | Sync         | Triggered when the CSE's configuration value is updated programmatically. |

=== "Logging"
	| Event Name | Payload     | Sync / Async | Description                                     |
	|:-----------|:------------|:-------------|:------------------------------------------------|
	| logWarning | Log message | Async        | Triggered when a warning log message is logged. |
	| logError   | Log message | Async        | Triggered when an error log message is logged.  |

=== "Console"
	| Event Name   | Payload   | Sync / Async | Description                                     |
	|:-------------|:----------|:-------------|:------------------------------------------------|
	| consoleInput | Key input | Sync         | Triggered when a key is pressed in the console. |


### Resource  Events

| Event Name          | Payload         | Sync / Async | Description                                                                |
|:--------------------|:----------------|:-------------|:---------------------------------------------------------------------------|
| retrieveResource    | Resource        | Async        | Triggered when a resource is retrieved.                                    |
| createResource      | Resource        | Async        | Triggered when a resource is created.                                      |
| updateResource      | Resource        | Async        | Triggered when a resource is updated.                                      |
| deleteResource      | Resource        | Async        | Triggered when a resource is deleted.                                      |
| expireResource      | Resource        | Async        | Triggered when a resource expires.                                         |
| changeResource      | Resource        | Async        | Triggered when a resource is changed, but not through an update operation. |
| createChildResource | Parent resource | Async        | Triggered when a child resource is created.                                |

	
### Request Related Events

=== "Request and Response"
	| Event Name       | Payload            | Sync / Async | Description                            |
	|:-----------------|:-------------------|:-------------|:---------------------------------------|
	| requestReceived  | Request structure  | Async        | Triggered when a request is received.  |
	| responseReceived | Response structure | Async        | Triggered when a response is received. |

=== "Notification"
	| Event Name       | Payload                     | Sync / Async | Description                                                                 |
	|:-----------------|:----------------------------|:-------------|:----------------------------------------------------------------------------|
	| notification     | None                        | Async        | Triggered when a notification happens, before sending it.                   |
	| acmeNotification | URL<br>Notification request | Sync         | Special event triggered when a notification targets a URL scheme "acme://". |


=== "CoAP"
	| Event Name       | Payload | Sync / Async | Description                                         |
	|:-----------------|:--------|:-------------|:----------------------------------------------------|
	| coapRetrieve     | None    | Async        | Triggered when a CoAP Retrieve request is received. |
	| coapCreate       | None    | Async        | Triggered when a CoAP Create request is received.   |
	| coapDelete       | None    | Async        | Triggered when a CoAP Delete request is received.   |
	| coapUpdate       | None    | Async        | Triggered when a CoAP Update request is received.   |
	| coapNotify       | None    | Async        | Triggered when a CoAP Notify request is received.   |
	| coapSendRetrieve | None    | Async        | Triggered when a CoAP Retrieve request is sent.     |
	| coapSendCreate   | None    | Async        | Triggered when a CoAP Create request is sent.       |
	| coapSendDelete   | None    | Async        | Triggered when a CoAP Delete request is sent.       |
	| coapSendUpdate   | None    | Async        | Triggered when a CoAP Update request is sent.       |
	| coapSendNotify   | None    | Async        | Triggered when a CoAP Notify request is sent.       |

=== "HTTP"
	| Event Name       | Payload | Sync / Async | Description                                          |
	|:-----------------|:--------|:-------------|:-----------------------------------------------------|
	| httpRetrieve     | None    | Async        | Triggered when an HTTP Retrieve request is received. |
	| httpCreate       | None    | Async        | Triggered when an HTTP Create request is received.   |
	| httpDelete       | None    | Async        | Triggered when an HTTP Delete request is received.   |
	| httpUpdate       | None    | Async        | Triggered when an HTTP Update request is received.   |
	| httpNotify       | None    | Async        | Triggered when an HTTP Notify request is received.   |
	| httpSendRetrieve | None    | Async        | Triggered when an HTTP Retrieve request is sent.     |
	| httpSendCreate   | None    | Async        | Triggered when an HTTP Create request is sent.       |
	| httpSendDelete   | None    | Async        | Triggered when an HTTP Delete request is sent.       |
	| httpSendUpdate   | None    | Async        | Triggered when an HTTP Update request is sent.       |
	| httpSendNotify   | None    | Async        | Triggered when an HTTP Notify request is sent.       |

=== "MQTT"
	| Event Name       | Payload | Sync / Async | Description                                          |
	|:-----------------|:--------|:-------------|:-----------------------------------------------------|
	| mqttRetrieve     | None    | Async        | Triggered when an MQTT Retrieve request is received. |
	| mqttCreate       | None    | Async        | Triggered when an MQTT Create request is received.   |
	| mqttDelete       | None    | Async        | Triggered when an MQTT Delete request is received.   |
	| mqttUpdate       | None    | Async        | Triggered when an MQTT Update request is received.   |
	| mqttNotify       | None    | Async        | Triggered when an MQTT Notify request is received.   |
	| mqttSendRetrieve | None    | Async        | Triggered when an MQTT Retrieve request is sent.     |
	| mqttSendCreate   | None    | Async        | Triggered when an MQTT Create request is sent.       |
	| mqttSendDelete   | None    | Async        | Triggered when an MQTT Delete request is sent.       |
	| mqttSendUpdate   | None    | Async        | Triggered when an MQTT Update request is sent.       |
	| mqttSendNotify   | None    | Async        | Triggered when an MQTT Notify request is sent.       |

=== "WebSocket"
	| Event Name     | Payload | Sync / Async | Description                                              |
	|:---------------|:--------|:-------------|:---------------------------------------------------------|
	| wsRetrieve     | None    | Async        | Triggered when a WebSocket Retrieve request is received. |
	| wsCreate       | None    | Async        | Triggered when a WebSocket Create request is received.   |
	| wsDelete       | None    | Async        | Triggered when a WebSocket Delete request is received.   |
	| wsUpdate       | None    | Async        | Triggered when a WebSocket Update request is received.   |
	| wsNotify       | None    | Async        | Triggered when a WebSocket Notify request is received.   |
	| wsSendRetrieve | None    | Async        | Triggered when a WebSocket Retrieve request is sent.     |
	| wsSendCreate   | None    | Async        | Triggered when a WebSocket Create request is sent.       |
	| wsSendDelete   | None    | Async        | Triggered when a WebSocket Delete request is sent.       |
	| wsSendUpdate   | None    | Async        | Triggered when a WebSocket Update request is sent.       |
	| wsSendNotify   | None    | Async        | Triggered when a WebSocket Notify request is sent.       |


### Registration Events

| Event Name                   | Payload                                                                                                    | Sync / Async | Description                                              |
|:-----------------------------|:-----------------------------------------------------------------------------------------------------------|:-------------|:---------------------------------------------------------|
| registeredToRegistrarCSE     | registrar config<br>registrar CSEBase resource<br>remoteCSE resource<br>local Registrar remoteCSE resource | Async        | Triggered when a CSE registers to a registrar CSE.       |
| deregisteredFromRegistrarCSE | registrar config<br>remoteCSE resource                                                                     | Async        | Triggered when a CSE deregisters from a registrar CSE.   |
| registreeCSEHasRegistered    | remoteCSE resource                                                                                         | Async        | Triggered when a registree's remoteCSE has registered.   |
| registreeCSEHasDeregistered  | remoteCSE resource                                                                                         | Async        | Triggered when a registree's remoteCSE has deregistered. |
| registeredToRemoteCSE        | remoteCSE resource                                                                                         | Async        | Triggered when a registree's registration is finished.   |
| csrUpdated                   | remoteCSE resource<br>updated attributes                                                                   | Async        | Triggered when a remoteCSE resource is updated.          |
| aeHasRegistered              | AE resource                                                                                                | Async        | Triggered when an AE resource has registered.            |
| aeHasDeregistered            | AE resource                                                                                                | Async        | Triggered when an AE resource has deregistered.          |


## Examples

### Defining an Event Handler with Decorators

The following example shows how to define an event handler for a plugin for resource creation events.

The [@eventHandler](https://api.acmecse.net/acmecse.helpers.EventManager.html#eventHandler){target="_new"} 
class decorator is used to indicate that the class contains decorated event handler methods. It is 
necessary to use this decorator on the class to enable the use of the 
[@onEvent](https://api.acmecse.net/acmecse.helpers.EventManager.html#onEvent){target="_new"} method decorator.

The [@onEvent](https://api.acmecse.net/acmecse.helpers.EventManager.html#onEvent){target="_new"} decorator is 
used to specify which event a method should handle. 
The [eventManager](https://api.acmecse.net/acmecse.runtime.EventManager.EventManager.html){target="_new"} provides 
properties for each of the available events that can be used with the 
[@onEvent](https://api.acmecse.net/acmecse.helpers.EventManager.html#onEvent){target="_new"} decorator.

The decorated method will be called whenever the specified event is triggered, and the event data 
will be passed as an argument to the method. In this example, the event handler simply prints a 
message to the console when a resource is created.

The event handler always receives an [EventData](https://api.acmecse.net/acmecse.helpers.EventManager.EventData.html){target="_new"} object as an argument, even if the event does not 
have a payload. 

```python title="Example Event Handler"
from acmecse.runtime.PluginSupport import plugin, eventHandler, onEvent, eventManager, EventData
from acmecse.runtime.Logging import Logging

@plugin
@eventHandler
class MyHandler():

	@onEvent(eventManager.createResource)
	def handle_create_resource(self, event: EventData) -> None:
		Logging.log(f'Event received: {event.name}. Resource created: {event.payload}')
```


### Defining an Event Handler with the EventManager API

It is also possible to assign event handlers programmatically using the
[EventManager](https://api.acmecse.net/acmecse.runtime.EventManager.EventManager.html){target="_new"} API. 
This can be useful if you want to define event handlers dynamically at runtime. This can be done
for any class, not just for plugins or assigned event handlers, but also for any other method.

You can add an event handler for any event using the 
[EventManager.addHandler()](https://api.acmecse.net/acmecse.helpers.EventManager.EventManager.html#addHandler){target="_new"} method.


```python title="Example Event Handler with EventManager API"
from acmecse.runtime.PluginSupport import eventManager, EventData
from acmecse.runtime.Logging import Logging

class MyClass():

	def __init__(self) -> None:
		# Register the event handler for an event
		eventManager.addHandler(eventManager.createResource, self.my_event_handler)

	# Define the event handler method
	def my_event_handler(self, event: EventData) -> None:
		Logging.log(f'Event received: {event.name}. Payload: {event.payload}')
```


### Custom Events 

You can add custom events to the ACME CSE using the
[addEvent()](https://api.acmecse.net/acmecse.helpers.EventManager.EventManager.html#addEvent){target="_new"} method, 
and then add an event handler for that event using the 
[addHandler()](https://api.acmecse.net/acmecse.helpers.EventManager.EventManager.html#addHandler){target="_new"} method
as described in the previous example. 

Please note, that custom events added with the 
[addEvent()](https://api.acmecse.net/acmecse.helpers.EventManager.EventManager.html#addEvent){target="_new"} method, 
cannot be used together with the [@onEvent](https://api.acmecse.net/acmecse.helpers.EventManager.html#onEvent){target="_new"} 
decorator if the event is defined after a handler method is defined and loaded. The reason for this is that decorators 
are evaluated at load time of a module, and the custom event will not be available at that time. 

```python title="Example Event Handler with EventManager API"
from acmecse.runtime.PluginSupport import plugin, eventHandler, eventManager, EventData
from acmecse.runtime.Logging import Logging

@plugin
@eventHandler
class MyHandler():

	def __init__(self) -> None:
		# Register the new event
		eventManager.addEvent('my_event')

		# Register the event handler for the new event
		eventManager.addHandler(eventManager.my_event,  self.my_event_handler)

		# Trigger the event with a payload
		eventManager.my_event('Hello, world!')

	# Define the event handler method
	def my_event_handler(self, event: EventData) -> None:
		Logging.log(f'Event received: {event.name}. Payload: {event.payload}')
```


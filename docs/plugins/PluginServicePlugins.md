# Service Plugins

In addition to the standard plugins described in the [Plugins Overview](PluginsOverview.md) and the [Plugin Example](PluginExample.md) documentation, the CSE also supports a special type of Plugins called *Services*.

Services can be used to define endpoints that are exposed internally and can be discovered and invoked by other plugins and class instances.

Services also provide an easy way to implement event handlers for the CSE's internal events, such as resource creation, modification, and deletion events. By defining a Service that handles these events, you can easily implement custom behavior that is triggered when these events occur.

## Defining a Service

To define a Service, you need to create a Plugin that has the
[acme.runtime.PluginSupport.Service](https://api.acmecse.net/acme.runtime.PluginSupport.Service.html){target="_new"} class as a base class. This class provides the necessary functionality to register the class as a Service and to define the endpoints that it exposes.

```python title="Example Service Class"
from acme.runtime.PluginSupport import plugin, Service

@plugin(tags=['example'])
class MyService(Service):
	...
```

## Service Endpoints

A Service can define endpoints that are exposed internally and can be discovered and invoked.

To define an endpoint, you can use the [@endpoint](https://api.acmecse.net/acme.helpers.PluginManager.html#endpoint){target="_new"} decorator on a methods of the Service class.

```python title="Example Service Endpoint"
from acme.runtime.PluginSupport import plugin, Service, endpoint

@plugin(tags=['example'])
class MyService(Service):

	@endpoint('service_endpoint')
	def endpoint_implementation(self, arg1: str, arg2: int) -> str:
		# Implementation of the endpoint
		return f'You called service_endpoint with arg1={arg1} and arg2={arg2}'
```

In this example, the `endpoint_implementation()` method is decorated with 
[@endpoint('service_endpoint')](https://api.acmecse.net/acme.helpers.PluginManager.html#endpoint){target="_new"}, which means that it is exposed as an endpoint with the name `service_endpoint`.

The specified endpoint name is also added as a property to the Service class to allow for easy access.


## Invoking Service Endpoints

To invoke the endpoint defined in the previous section, you can use the 
[PluginManager.callService()](https://api.acmecse.net/acme.helpers.PluginManager.PluginManager.html#callService){target="_new"} method. This method takes the endpoint name as the first argument, followed by one or more tags (or `None` to not filter) to optionally filter on the Service that provides the endpoint, and then any arguments that need to be passed to the endpoint.

The [callService()](https://api.acmecse.net/acme.helpers.PluginManager.PluginManager.html#callService){target="_new"} method will return the result(s) returned by the endpoint implementation(s) in a list of tuples. Each tuple contains the result returned by the endpoint and the *name* of the Service that provided the endpoint.		
If there are multiple Services that provide the same endpoint, all matching Services will be called and their results will be returned in the list. 


=== "Service Endpoint"
	```python title="Invoking a Service Endpoint (file:my_service.py)"
	from acme.runtime.PluginSupport import plugin, Service, endpoint

	@plugin(tags=['example'])
	class MyService(Service):

		@endpoint('service_endpoint')
		def endpoint_implementation(self, arg1: str, arg2: int) -> str:
			# Implementation of the endpoint
			return f'You called service_endpoint with arg1={arg1} and arg2={arg2}'
	```

=== "Calling the Service Endpoint"
	```python title="Invoking the Service Endpoint"

	# Somewhere else in the code, we can call the service endpoint like this:
	from acme.runtime.PluginSupport import pluginManager

	result = pluginManager.callService('service_endpoint', tags='example', arg1='value1', arg2=42)
	print(result) # -> [('You called service_endpoint with arg1=value1 and arg2=42', 'plugins.my_service')]
	```

!!! Note
	If there are multiple Services that provide the same endpoint, you can use the `tags` parameter to specify which Service to call. If there are multiple Services that match the specified tags, all matching Services will be called. If no Service matches the specified endpoint and tags, a `ValueError` will be raised.


## Event Handling

In addition a Service class can define event handler methods to handle internal CSE events. To define an event handler, you can use the [@onEvent](https://api.acmecse.net/acme.helpers.EventManager.html#onEvent){target="_new"} decorator on a method of the Service class.

Any plugin can define event handlers, not just Services. The 
[`Service`](https://api.acmecse.net/acme.runtime.PluginSupport.Service.html){target="_new"} class is just a convenience base class that provides additional functionality for defining and exposing service endpoints

The following example shows how to define an event handler for resource creation events.

```python title="Example Event Handler"
from acme.runtime.PluginSupport import plugin, Service, onEvent, eventManager, EventData

@plugin(tags=['example'])
class MyService(Service):

	@onEvent(eventManager.createResource)
	def handle_create_resource(self, event: EventData) -> None:
		print(f'Event received: {event.name}. Resource created: {event.payload}')
```

For further examples on how to define event handlers, see the [Event System](../development/EventSystem.md) documentation.

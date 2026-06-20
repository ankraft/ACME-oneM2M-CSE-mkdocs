# Service Plugins

In addition to the standard plugins described in the [Plugins Overview](PluginsOverview.md) and the [Plugin Example](PluginExample.md) documentation, the CSE also supports a special type of Plugins called *Services*.

Services can be used to define endpoints that are exposed internally and can be discovered and invoked by other plugins and class instances.

Services also provide an easy way to implement event handlers for the CSE's internal events, such as resource creation, modification, and deletion events. By defining a Service that handles these events, you can easily implement custom behavior that is triggered when these events occur.

## Defining a Service

To define a Service, you need to create a Plugin that has the
[acmecse.helpers.PluginManager.Service](https://api.acmecse.net/acmecse.helpers.PluginManager.Service.html){target="_new"} class as a base class. This class provides the necessary functionality to register the class as a Service and to define the endpoints that it exposes.  
The package `acmecse.runtime.PluginSupport` conveniently provides all necessary imports for defining a Service, so you can simply import everything from that package to get access to the `Service` class and the necessary decorators.

```python title="Example Service Class"
from acmecse.runtime.PluginSupport import plugin, Service

@plugin(tags=['example'])
class MyService(Service):
	...
```

## Service Endpoints

A Service can define endpoints that are exposed internally and can be discovered and invoked.

To define an endpoint, you can use the [@endpoint](https://api.acmecse.net/acmecse.helpers.PluginManager.html#endpoint){target="_new"} decorator on a methods of the Service class.

```python title="Example Service Endpoint"
from acmecse.runtime.PluginSupport import plugin, Service, endpoint

@plugin(tags=['example'])
class MyService(Service):

	@endpoint('service_endpoint')
	def endpoint_implementation(self, arg1: str, arg2: int) -> str:
		# Implementation of the endpoint
		return f'You called service_endpoint with arg1={arg1} and arg2={arg2}'
```

In this example, the `endpoint_implementation()` method is decorated with 
[@endpoint('service_endpoint')](https://api.acmecse.net/acmecse.helpers.PluginManager.html#endpoint){target="_new"}, which means that it is exposed as an endpoint with the name `service_endpoint`.

The specified endpoint name is also added as a property to the Service class to allow for easy access.



## Discovering Services and Endpoints

Services and their endpoints can be discovered using the [PluginManager](https://api.acmecse.net/acmecse.helpers.PluginManager.PluginManager.html){target="_new"} API. The PluginManager provides methods to get a list of all registered Services, to get a specific Service by name or by tags, and to get the endpoints provided by a Service.

To get a list of all registered Services, you can use the [services()](https://api.acmecse.net/acmecse.helpers.PluginManager.PluginManager.html#services){target="_new"} method:

```python title="Get All Services"
from acmecse.runtime.PluginSupport import pluginManager

services = pluginManager.services()
print(services) # ->  [('plugins.MyService', ['example'], {})]
``` 

The returned list contains tuples with the name of the Service, its tags, and any additional metadata provided by the Service. In this example, there is one Service registered with the name `plugins.MyService`, the tag `example`, and no additional metadata.

To get a specific Service you can add a tag or list of tags to filter the Services by. For example, to get the Service with the tag `example`, you can use the following code:

```python title="Get Service by Tag"
from acmecse.runtime.PluginSupport import pluginManager

service = pluginManager.service('example')
print(service) # ->  [('plugins.MyService', ['example'], {})]
```

To get the endpoints provided by a Service, you can use the [endpoints()](https://api.acmecse.net/acmecse.helpers.PluginManager.PluginManager.html#endpoints){target="_new"} method of the [PluginManager](https://api.acmecse.net/acmecse.helpers.PluginManager.PluginManager.html){target="_new"} class. For example, to get the endpoints provided by the `MyService` Service, you can use the following code:

```python title="Get Endpoints of a Service"
from acmecse.runtime.PluginSupport import pluginManager

endpoints = pluginManager.endpoints('plugins.MyService')
print(endpoints) # ->   [('service_endpoint', <Signature (arg1: str, arg2: int) -> str>)]
```

This will return a list of tuples with the name of the endpoint and its signature. In this example, there is one endpoint provided by the `MyService` Service with the name `service_endpoint` and the signature `(arg1: str, arg2: int) -> str`. The signature is provided as a `Signature` object from the `inspect` module, which allows you to easily get information about the endpoint's parameters and return type.


## Calling Service Endpoints

To call the endpoint defined in the previous section, you can use the 
[PluginManager.callEndpoints()](https://api.acmecse.net/acmecse.helpers.PluginManager.PluginManager.html#callEndpoints){target="_new"} method. This method takes the endpoint name as the first argument, followed by one or more tags (or `None` to not filter) to optionally filter on the Service that provides the endpoint, and then any arguments that need to be passed to the endpoint.


The [callEndpoints()](https://api.acmecse.net/acmecse.helpers.PluginManager.PluginManager.html#callEndpoints){target="_new"} method will return the result(s) returned by the endpoint implementation(s) in a list of tuples. Each tuple contains the result returned by the endpoint, the *name* of the Service that provided the endpoint, and any additional servie metadata.  
If there are multiple Services that provide the same endpoint, all matching endpoints will be called and their results will be returned in the list. 


=== "Providing a Service Endpoint"
	```python 
	# We assume that the filename of this code is "plugins/my_service.py"

	from acmecse.runtime.PluginSupport import plugin, Service, endpoint

	@plugin(tags=['example'])
	class MyService(Service):

		@endpoint('service_endpoint')
		def endpoint_implementation(self, arg1: str, arg2: int) -> str:
			# Implementation of the endpoint
			return f'You called service_endpoint with arg1={arg1} and arg2={arg2}'
	```

=== "Calling the Service Endpoint"
	```python 

	# Somewhere else in the code, we can call the service endpoint like this:
	from acmecse.runtime.PluginSupport import pluginManager

	result = pluginManager.callEndpoints('service_endpoint', tags='example', arg1='value1', arg2=42)
	print(result) # -> [('You called service_endpoint with arg1=value1 and arg2=42', 'plugins.my_service', {})]
	```

!!! Note
	If there are multiple Services that provide the same endpoint, you can use the `tags` parameter to specify which Service to call. If there are multiple Services that match the specified tags, all matching Services will be called. If no Service matches the specified endpoint and tags, a [PluginNotFoundError](https://api.acmecse.net/acmecse.helpers.PluginManager.PluginNotFoundError.html){target="_new"} exception will be raised.

TODO call a single endpoint


## Service Metadata

When defining a Service, you can also provide additional metadata about the Service by setting the class attribute `_service_metadata_` to a dictionary containing the desired metadata. This metadata is then provided when invoking the Service's endpoints.

```python title="Example Service Metadata"
from acmecse.runtime.PluginSupport import plugin, Service, endpoint

@plugin(tags=['example'])
class MyService(Service):

	_service_metadata_ = {
		'description': 'This is an example service that demonstrates how to define service metadata.',
		'version': '1.0.0',
	}
	
	...
```

The content of the `_service_metadata_` dictionary is entirely up to the plugin developer. It can contain any information that might be useful for other plugins or for the users of the Service. For example, it could contain a description of the Service, its version, the author, or any other relevant information.	


TODO get metadata when call services




## Event Handling

In addition a Service class can define event handler methods to handle internal CSE events. To define an event handler, you can use the [@onEvent](https://api.acmecse.net/acmecse.helpers.EventManager.html#onEvent){target="_new"} decorator on a method of the Service class.

Any plugin can define event handlers, not just Services. 

The following example shows how to define an event handler for resource creation events.

```python title="Example Event Handler"
from acmecse.runtime.PluginSupport import plugin, eventHandler, Service, onEvent, eventManager, EventData

@plugin(tags=['example'])
@eventHandler
class MyService(Service):

	@onEvent(eventManager.createResource)
	def handle_create_resource(self, event: EventData) -> None:
		print(f'Event received: {event.name}. Resource created: {event.payload}')
```

For further examples on how to define event handlers, see the [Event System](../development/EventSystem.md) documentation.

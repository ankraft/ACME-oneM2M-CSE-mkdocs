# Supported oneM2M Functionalities

This article lists the supported oneM2M resource types and related functionalities of the ACME CSE


## oneM2M Specification Conformance

The CSE implementation successfully passes all the relevant oneM2M test cases for the supported resource types, attributes and behaviours.

## Release Versions

The ACME CSE supports oneM2M release **1 - 4** and the upcoming release **5** for the supported resource types and functionalities listed below. 

## CSE Types

The ACME CSE supports the following CSE types:

- IN-CSE
- MN-CSE
- ASN-CSE

---

## Resource Types

The ACME CSE supports the following oneM2M resource types. Some resource types 
are not yet fully implemented, and some features are experimental.

<figure markdown="1">
![UML Class Diagram of the ACME CSE Resources](../images/resources_uml.png#only-light){data-gallery="light"}
![UML Class Diagram of the ACME CSE Resources](../images/resources_uml-dark.png#only-dark){data-gallery="dark"}
<figcaption>UML Class Diagram of the Supported oneM2M Resources Types</figcaption>
</figcaption>
</figure>


### Entities

**CSEBase (CB)**

:	The CSEBase resource type is fully supported.


**Application Entity (AE)**

:	The Application Entity resource type is supported, except for some of the "S" registration related functionalities.



**RemoteCSE (CSR)**

:	The ACME CSE supports CSE registrations via the Mcc reference point, and 
	Service Provder registrations via the Mcc' reference point. 
	In addtion announced resources, synchronization, and transit requests that 
	target resources on remote CSE's are supported, as well.


### Security

**Access Control Policy (ACP)**

:	In addition to the basic ACP functionality, the ACME CSE supports the following ACP features:

	- Attribute-based access control
	- accessControlWindow

	The following attributes are not supported because the linked-to resource types are not yet supported.

	- `authorizationDecisionResourceIDs` / &lt;authorizationDecision>
	- `authorizationPolicyResourceIDs`/ &lt;authorizationPolicy>
	- `authorizationInformationResourceIDs` / &lt;authorizationInformation>



### Data Management

**Container (CNT)**

:	The Container resource type is fully supported.


**ContentInstance (CIN)**

:	The ContentInstance resource type is fully supported.

	Values with the following data types are supported for the *content* attribute:

	- string
	- integer  
	- float
	- boolean
	- list
	- dictionary / JSON object 


**FlexContainer (FCNT)**

:	The ACME CSE fully supports the FlexContainer resource type.

	It is possible to use the predefined FlexContainer Specializations of [oneM2M TS-0023](https://specifications.onem2m.org/ts-0023/latest){target=_new}, *AllJoin*, oneM2M's *GenericInterworking*, or to define custom FlexContainerSpecializations.

	See [Attribute Policies](../development/AttributePolicies.md) for further details.


**FlexContainerInstance (FCI)**

:	This is an experimental implementation of the draft FlexContainerInstance specification.


**TimeSeries (TS)**

:	The ACME CSE supports missing data detection and the respective notifications.


**TimeSeriesInstance (TSI)**

:	The TimeSeriesInstance resource type is fully supported, except for the *dataGenerationTime*
	attribute, which is only supported with absolute timestamps.

	Values with the following data types are supported for the *content* attribute:

	- string 
	- integer
	- float 
	- boolean 
	- list
	- dictionary / JSON object  


### Subscription and Notification

**CrossResourceSubscription (CRS)**

:	Besides the standard CrossResourceSubscription functionality the ACME CSE implements an experimental feature to support an *eventEvaluationMode* to react on missing events.


**Subscription (SUB)**

:	The ACME CSE supports notifications via direct url or an AE's Point-of-Access (POA). 
	Further,  BatchNotifications, attributes, notification statistics, and operation 
	monitoring are supported.   


**NotificationTargetSelfReference (NTSR)**

:	The NotificationTargetSelfReference virtual resource type is fully supported.

	This resource type is used to support self-removal of notification targets.
	It is used together with the following resource types.


**notificationTargetPolicy (NTP)**

:	The NotificationTargetPolicy resource type is fully supported.

	This resource type is used to configure the policies that are used for notification
	target self-removal.

	The CSE automatically creates a system "Default" &lt;NTP> resource during startup, which is used for all notification targets that do not have a specific &lt;NTP> resource assigned.


**notificationTargetMgmtPolicyRef (NTPR)**

:	The NotificationTargetManagementPolicyReference resource type is fully supported.

	This resource type is used to reference the &lt;NTP> resource that is used during the self-removal of notification targets.


**policyDeletionRules (PDR)**

:	The PolicyDeletionRules resource type is partly supported.

	This resource type specifies the policies for the self-removal of notification targets.

	The ACME CSE currently only supports scheduling policies, but not the geo-location policies.


### Device Management

**Management Objects**

:	The ACME CSE supports the following management objects:

| Management Objects       | Management Objects     | Management Objects |
|--------------------------|------------------------|--------------------|
| AreaNwkDeviceInfo (ANDI) | DeviceInfo (DVI)       | Reboot (REB)       |
| AreaNwkInfo (ANI)        | EventLog (EVL)         | SIM (SIM)          |
| Battery (BAT)            | Firmware (FWR)         | Software (SWR)     |
| Credentials (CRDS)       | Memory (MEM)           | WifiClient (WIFIC) |
| DataCollect (DATC)       | MobileNetwork (MNWK)   |                    |
| DeviceCapability (DVC)   | MyCertFileCred (NYCFC) |                    |


**Node (NOD)**

:	The Node resource type is fully supported.


### Communication

**PollingChannel (PCH)**

:	Request and notification long-polling via the *pcu* (pollingChannelURI) 
	virtual child resource are supported.  
	*requestAggregation* functionality to retrieve multiple requests in one 
	polling request is supported as well.


**Request (REQ)**

:	The ACME CSE supports blocking, and synchronous and asynchronous non-blocking
	 requests, which are managed through &lt;request> resources.


### Group Management

**Group (GRP)**

:	The ACME CSE supports requests via the *fopt* (fanOutPoint) virtual resource.  
	Remote resources may be members of a group.



### Automation

**Action (ACTR)**

:	The `input` attribute of the &lt;action> resource type is not supported yet.


**Dependency (DEPR)**

:	The Dependency resource type is fully supported.


### Semantics

**SemanticDescriptor (SMD)**

:	The ACME CSE supports semantic queries and discovery for resources with semantic descriptors.

	At the moment, the following presentation formats are supported:

	- RDF/XML
	- JSON-LD
	- Turtle



### Location Management

**LocationPolicy (LCP)**

:	Only *device based* location policy is supported. 
	The LCP's *cnt* stores geo-coordinates and geo-fencing results.


### Time Management

**Schedule (SCH)**

:	The ACME CSE supports scheduling for various of its functions, such as CSE 
	communication windows, and resource types, such as the &lt;node>, 
	&lt;subscription> and &lt;crossResourceSubscription> resource types.


**TimeSyncBeacon (TSB)**

:	The ACME CSE supports the TimeSyncBeacon resource type. The functionality is currently
	only **experimental** and might change according to specification changes.




## Service Functionalities

The following oneM2M service functionalities are supported.

**AE Registration**

:	The ACME CSE supports AE registration and deregistrations via the Mca reference point.


**Blocking and Non-Blocking Requests**

:	Blocking requests are the common way for an AE to interact with the CSE.
	They are also used for CSE-to-CSE communication via the  and Mcc' reference points.

	To avoid blocking the AE, the ACME CSE supports non-blocking requests in 
	synchronous and asynchronous mode as well.


**Delayed Request Execution**

:	The ACME CSE supports delayed request execution via the *Operation Execution Timestamp* request parameter.


**Discovery**

:	The ACME CSE supports normal retrieval and discovery of resources via the filterCriteria 
	and discoveryResultType parameters.


**Geo-Query**

:	The ACME CSE supports location and geometry queries and location-based discovery.


**Location Management**

:	Only *device based*, and no *network based* location policies are supported. 


**Long Polling**

:	*Long Polling* is supported for *request unreachable* AEs and CSEs through *pollingChannel* resources.

	This mechanism is used for sending notifications and other requests to AEs and CSEs that are not directly reachable 
	via the Mca, Mcc or Mcc' reference points, e.g. because of firewalls or NATs.


**No-Response Requests**

:	No-response requests are supported for requests that do not require a response. This is especially
	useful for constrained devices that do not need to wait for a response, or cannot handle
	possible error responses.

	!!! info "Experimental Feature"
		This feature is still experimental and might change according to specification changes.


**Notifications**

:	The ACME CSE supports notifications via direct url, or an AE's or CSE's Point-of-Access (POA). 
	Further,  BatchNotifications, attributes, notification statistics, and operation 
	monitoring are supported.


**Notification Event Types**

:	The following notification event types for Subscriptions are supported:

| Notification Event Type | Notification Event Type | Notification Event Type |
|:------------------------|:------------------------|:------------------------|
| resourceUpdate          | retrieveCNTNoChild      | blockingUpdate          |
| createDirectChild       | triggerReceivedForAE    |                         |
| deleteDirectChild       | missingData             |                         |


**Partial Retrieve**

:	Partial retrieve of individual resource attributes is supported.


**Remote CSE Registration**

:	The ACME CSE supports CSE registrations via the Mcc and Mcc' reference points. 
	In addtion announced resources, synchronization, and transit requests that 
	target resources on remote CSE's are supported.


**Request Expiration**

:	The *Request Expiration Timestamp* request parameter is supported.


**Request Forwarding**

:	Forwarding requests from one CSE to another is supported.


**Request and Resource Validations**

:	All requests and resources received via the Mca, Mcc and Mcc' reference points are validated.


**Resource Addressing**

:	*CSE-Relative*, *SP-Relative* and *Absolute* as well as hybrid addressing are supported.


**Resource Announcements**

:	Announcements of resources are supported via the Mcc and Mcc' reference points.
	Resources are announced under a CSEBaseAnnc resource on the target CSE (R4 feature).  
	Bi-directional update and attribute syncronization is supported as well.


**Resource Expiration**

:	Resources are automatically deleted after the expiration time has passed.


**Result Content Types**

:	The following result contents are implemented for standard oneM2M requests and discovery:

	| Discovery Type                         | RCN | Discovery Type              | RCN |
	|:---------------------------------------|:---:|:----------------------------|:---:|
	| nothing                                |  0  | child-resource-references   |  6  |
	| attributes                             |  1  | original-resource           |  7  |
	| hierarchical address                   |  2  | child-resources             |  8  |
	| hierarchical address + attributes      |  3  | modified attributes         |  9  |
	| attributes + child-resources           |  4  | semantic content            | 10  |
	| attributes + child-resource-references |  5  | discovery result references | 11  | 


**Result Expiration**

:	The *Result Expiration Timestamp* request parameter is supported.


**Semantics**

:	Basic support for semantic descriptors, semantic queries and discovery is available.


**Service Provider Registration**

:	Registrations between Service Provider IN-CSEs are supported via the Mcc'
	reference point. All the functionalities of the Mcc reference point are
	supported, such as resource announcements, synchronization, and transit
	requests that target resources on remote oneM2M service provider CSEs.


**Subscriptions**

:	Subscriptions are supported, including batch notification, and resource type and attribute filtering.


**Time Synchronization**

:	The ACME CSE supports time synchronization via the *timeSyncBeacon* resource type.


**TimeSeries Data Handling**

:	TimeSeries data handling is supported, including missing data detection, monitoring and notifications.



## Protocols Bindings

The following *Protocol Bindings* are supported.  
It is possible to enable only one binding, or to use any combination of them for a CSE instance.

**CoAP**

:	The CoAP protocol is supported.  
	CoAP over DTLS is not yet implemented.

	!!! Info "Release 5"
		The current implementation only support the CoAP binding specification for oneM2M Release 1-4. 
		The upcoming Release 5 specification is not yet supported.

**http**

:	The http protocol is supported, including TLS (https) and CORS support. 

	*[CORS]: Cross-Origin Resource Sharing
	
	The *basic* and *bearer* authentication methods are supported.
	
	!!! info "Experimental Feature"
		Some libraries and embedded chipsets do not support the DELETE method in http/1.0 (this method was only added in http/1.1). The ACME CSE therefore supports DELETE requests via the PATCH method, that is available in many http/1.0 implementations.  

		See also the [allowPatchForDelete](../setup/Configuration-http.md#general-settings) configuration setting.

**mqtt**

:	The mqtt protocol is supported, including TLS (mqtts) support.

	This protocol binding requires a separate MQTT broker to be installed and running.

	Basic username/password authentication with an mqtt broker is supported as well.

**WebSocket**

:	The WebSocket protocol is supported, including TLS (wss) support   

	!!! info  "Experimental Feature"
		Besides the standard oneM2M WebSocket binding, the ACME CSE supports an	[experimental extension](../howtos/ExperimentalWebSocketBinding.md) to better support notifications.


## Serialization Types

The following serialization types for requests and responses are supported. 

!!! info
	The XML serialization is not supported.  
	Though it is part of the oneM2M standard, it is not planned to be implemented in the ACME CSE because XML is only very rarely used in real-world IoT deployments.


**JSON**

:	In addition to the common JSON syntax, C-style and Python-style comments are supported as well:

	- Block comment:  `/* ... */`
	- End-of-line comment: `// ...` and `# ...`

**CBOR**

:	CBOR is a [binary serialization](https://cbor.io){target=_new} format.  
	Messages are fairly smaller and faster to process than JSON messages.

	*[CBOR]: Concise Binary Object Representation



## Experimental Functionalities

These features are prove-of-concept implementations of new and currently 
discussed oneM2M functionalities. They are not yet part of the oneM2M standard.

**Enhanced CSR functionality**

:	Support for a new R5 *eventEvaluationMode* to react on missing events.


**Subscription References**

:	Support for subscription references for resource instead of direct subscriptions.


**Advanced Queries**

:	Experimental implementation of a new query language to support enhanced query capabilities.


**Simplified Time Synchronization**

:	Experimental implementation of a simplified time synchronization mechanism.


**Support for DELETE requests for http/1.0**

:	Using [PATCH requests](../setup/Configuration-http.md#general-settings) to emulate DELETE requests for http/1.0 clients.


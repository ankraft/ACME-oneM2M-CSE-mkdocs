# Supported Resource Types and Functionalities

This article lists the supported oneM2M resource types, functionalities, and additional runtime features of the ACME CSE.


## oneM2M Features

### oneM2M Specification Conformance

The CSE implementation successfully passes all the relevant oneM2M test cases for the supported resource types, attributes and behaviours.

### Release Versions

The ACME CSE supports oneM2M release 1 - 4 and the upcoming release 5 for the supported resource types and functionalities listed below. 

### CSE Types

The ACME CSE supports the following CSE types:

- IN-CSE
- MN-CSE
- ASN-CSE


### Supported Resource Types

The ACME CSE supports the following oneM2M resource types. Some resource types 
are not yet fully implemented, and some features are experimental.

**Access Control Policy (ACP)**
:	In addition to the basic ACP functionality, the ACME CSE supports the following ACP features:

	- Attribute-based access control
	- accessControlWindow

	Not supported are the following attributes because the linked-to resource types are not supported.

	- `authorizationDecisionResourceIDs` / &lt;authorizationDecision>
	- `authorizationPolicyResourceIDs`/ &lt;authorizationPolicy>
	- `authorizationInformationResourceIDs` / &lt;authorizationInformation>


**Action (ACTR)**
:	The `input` attribute is not supported yet.


**Application Entity (AE)**
:	The Application Entity resource type is supported, except for some of the "S" registration related functionalities.


**Container (CNT)**


**ContentInstance (CIN)**


**CrossResourceSubscription (CRS)**
:	Besides the normal CrossResourceSubscription functionality the ACME CSE implements an experimental feature to support an *eventEvaluationMode* to react on missing events.


**CSEBase (CB)**


**Dependency (DEPR)**


**FlexContainer (FCNT)**
:	The ACME CSE fully supports the FlexContainer resource type.

	It is possible to use the predefined FlexContainerSpecializations of [oneM2M TS-0023](https://specifications.onem2m.org/ts-0023/latest){target=_new}, *AllJoin*, oneM2M's *GenericInterworking*, or to define custom FlexContainerSpecializations.

	See [Attribute Policies](../development/AttributePolicies.md) for further details.


**FlexContainerInstance (FCI)**
:	This is an experimental implementation of the draft FlexContainerInstance specification.	


**Group (GRP)**
:	The ACME CSE supports requests via the *fopt* (fanOutPoint) virtual resource.
	Groups may contain remote resources.


**LocationPolicy (LCP)**
:	Only *device based* location policy is supported. 
	The LCP's *cnt* stores geo-coordinates and geo-fencing results.


**Management Objects**
:	The ACME CSE supports the following management objects:

	| Management Objects       | Management Objects     |
	|--------------------------|------------------------|
	| AreaNwkDeviceInfo (ANDI) | Memory (MEM)           |
	| AreaNwkInfo (ANI)        | MobileNetwork (MNWK)   |
	| Battery (BAT)            | MyCertFileCred (NYCFC) |
	| DataCollect (DATC)       | Reboot (REB)           |
	| DeviceCapability (DVC)   | SIM (SIM)              |
	| DeviceInfo (DVI)         | Software (SWR)         |
	| EventLog (EVL)           | WifiClient (WIFIC)     |
	| Firmware (FWR)           |                        |


**Node (NOD)**


**PollingChannel (PCH)**
:	Request and Notification long-polling via the *pcu* (pollingChannelURI) 
	virtual child resource is supported. *requestAggregation* functionality is supported as well.


**RemoteCSE (CSR)**
:	The ACME CSE supports CSE registrations via the Mcc reference point. In addtion announced resources, synchronization, and transit requests that target resources on remote CSE's are supported, as well.


**Request (REQ)**
:	The ACME CSE supports blocking, and synchronous and asynchronous non-blocking requests, which are managed through &lt;request> resources.
	

**Schedule (SCH)**
:	The ACME CSE supports scheduling for various aspects like CSE communication, nodes, subscriptions and crossResourceSubscriptions.


**SemanticDescriptor (SMD)**


**Subscription (SUB)**

:	The ACME CSE supports notifications via direct url or an AE's Point-of-Access (POA). Further,  BatchNotifications, attributes, notification statistics, and operation monitoring are supported.   


**TimeSeries (TS)**
:	The ACME CSE supports missing data detection, monitoring and missing data notifications.


**TimeSeriesInstance (TSI)**
:	The ACME CSE supports the *dataGenerationTime* attribute, but only with absolute timestamps.


**TimeSyncBeacon (TSB)**
:	The ACME CSE supports the TimeSyncBeacon resource type. The functionality is currently only experimental and might change according to specification changes.



### Protocols Bindings

The following Protocol Bindings are supported:

| Protocol Binding | Remark                                                                                                                                                                                           |
|:-----------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| http             | incl. TLS (https) and CORS support. *basic* and *bearer* authentication. <br/>Experimental: Using [PATCH to replace missing DELETE in http/1.0](../setup/Configuration-http.md#general-settings) |
| mqtt             | incl. mqtts                                                                                                                                                                                      |
| WebSocket        | incl. TLS (wss) support                                                                                                                                                                          |

The supported bindings can be used together, and combined and mixed in any way.

!!! info 
	The CoAP protocol binding is not yet supported, but planned for a future release.


### Serialization Types
The following serialization types are supported:

| Serialization Type | Remark                                                                                                         |
|:-------------------|:---------------------------------------------------------------------------------------------------------------|
| JSON               | In addition to normal JSON syntax, C-style comments ("//...", "#..." and "/\* ... \*/") are supported as well. |
| CBOR               | CBOR is a [binary serialization](https://cbor.io){target=_new} format.                                         |

The supported serializations can be used together, e.g. between different or even with the same entity.


!!! info
	The XML serialization is not supported. Though it is part of the oneM2M standard, it is not planned to be implemented in the ACME CSE because of the limited use of XML in IoT environments.


### Result Content Types

The following result contents are implemented for standard oneM2M requests and discovery:

| Discovery Type                         | RCN |
|:---------------------------------------|:---:|
| nothing                                | 0   |
| attributes                             | 1   |
| hierarchical address                   | 2   |
| hierarchical address + attributes      | 3   |
| attributes + child-resources           | 4   |
| attributes + child-resource-references | 5   |
| child-resource-references              | 6   |
| original-resource                      | 7   |
| child-resources                        | 8   |
| modified attributes                    | 9   |
| semantic content                       | 10  |
| discovery result references            | 11  |


### oneM2M Service Functions

The following oneM2M service functions are supported:

| Functionality                 | Remark                                                                                                                                     |
|:------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------|
| AE registration               |                                                                                                                                            |
| Blocking requests             | This is the common way to interact with the CSE.                                                                                           |
| Delayed request execution     | Supported via the *Operation Execution Timestamp* request attribute.                                                                       |
| Discovery                     |                                                                                                                                            |
| Geo-query                     | Location and geometry queries                                                                                                              |
| Location                      | Only *device based, and no *network based* location policies are supported.                                                                |
| Long polling                  | Long polling for request unreachable AEs and CSEs through &lt;pollingChannel>.                                                             |
| Non-blocking requests         | Non-blocking synchronous and asynchronous, and flex-blocking, incl. *Result Persistence*.                                                  |
| Notifications                 | E.g. for subscriptions and non-blocking requests.                                                                                          |
| Partial Retrieve              | Support for partial retrieve of individual resource attributes.                                                                            |
| Remote CSE registration       |                                                                                                                                            |
| Request expiration            | The *Request Expiration Timestamp* request attribute                                                                                       |
| Request forwarding            | Forwarding requests from one CSE to another.                                                                                               |
| Request parameter validations | Validation of all requests and resources.                                                                                                  |
| Resource addressing           | *CSE-Relative*, *SP-Relative* and *Absolute* as well as hybrid addressing are supported.                                                   |
| Resource announcements        | Under the CSEBaseAnnc resource (R4 feature), including bi-directional update sync.                                                         |
| Resource expiration           |                                                                                                                                            |
| Resource validations          |                                                                                                                                            |
| Result expiration             | The *Result Expiration Timestamp* request attribute. Result timeouts for non-blocking requests depend on the resource expiration interval. |
| Semantics                     | Basic support for semantic descriptors and semantic queries and discovery.                                                                 |
| Standard oneM2M requests      | CREATE, RETRIEVE, UPDATE, DELETE, NOTIFY                                                                                                   |
| Subscriptions                 | Incl. batch notification, and resource type and attribute filtering.                                                                       |
| Time Synchronization          |                                                                                                                                            |
| TimeSeries data handling      | Incl. missing data detection, monitoring and notifications.                                                                                |


###  Notification Event Types

The following notification event types for Subscriptions are supported:

| Notification Event Types |
|:-------------------------|
| resourceUpdate           |
| resourceUpdate           |
| createDirectChild        |
| deleteDirectChild        |
| retrieveCNTNoChild       |
| triggerReceivedForAE     |
| blockingUpdate           |
| missingData              |



## Additional CSE Runtime Features

In addition to the oneM2M standard functionalities, the ACME CSE implements additional features to help with deployments and to support new and experimental features.

| Functionality         | Remark                                                                                                                       |
|:----------------------|:-----------------------------------------------------------------------------------------------------------------------------|
| HTTP CORS             | Support for *Cross-Origin Resource Sharing* to support http(s) redirects.                                                    |
| HTTP Authorization    | Basic support for *basic* and *bearer* (token) authorization.                                                                |
| HTTP WSGI             | Support for the Python *Web Server Gateway Interface* to improve integration with a reverse proxy or API gateway, ie. Nginx. |
| Text Console          | Control and manage the CSE, inspect resources, run scripts in a text console.                                                |
| Text UI               | Text-based UI to inspect resources and requests, configurations, stats, manage resources, and more                           |
| Testing: Upper Tester | Basic support for the Upper Tester protocol defined in TS-0019, and additional command execution support.                    |
| Request Recording     | Record requests to and from the CSE to learn and debug requests over Mca and Mcc.                                            |
| Script Interpreter    | Lisp-based scripting support to extent functionalities, implement simple AEs, prototypes, test, ...                          |
| Web UI                | A simple Web UI that displays the oneM2M resource tree and offers a simple REST UI.                                          |



### Runtime Environments
The ACME CSE runs at least on the following runtime environments:

| Runtime Environment | Remark                                                                                |
|:--------------------|:--------------------------------------------------------------------------------------|
| Generic Linux       | Including [Raspberry Pi OS (32bit)](../howtos/RaspberryPi.md) on Raspberry Pi 3 and 4. |
| Mac OS              | Intel and Apple silicon.                                                              |
| MS Windows          |                                                                                       |
| Jupyter Notebooks   | ACME CSE can be run headless inside a Jupyter Notebook.                               |


### Database Bindings

The following database bindings are supported:

| Database Binding  | Remark                               |
|:------------------|:-------------------------------------|
| PostgreSQL        |                                      |
| TinyDB in-memory  | Fast and simple in-memory database.  |
| TinyDB file-based | Fast and simple file-based database. |



### Experimental CSE Features
These features are prove-of-concept implementations of new and currently discussed oneM2M features. They are not yet part of the oneM2M standard.

| Functionality                            | Remark                                                                                     |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------|
| Enhanced CSR functionality               | Support of new *eventEvaluationMode* to react in missing events                            |
| Subscription References                  | Support for subscription references for resource instead of direct subscriptions.          |
| Advanced Queries                         | Experimental implementation of a new query language to support enhanced query capabilities |
| Simplified Time Synchronization          | Experimental implementation of a simplified time synchronization mechanism.                |
| Support for DELETE requests for http/1.0 | Using PATCH requests to emulate DELETE requests for http/1.0 clients.                      |


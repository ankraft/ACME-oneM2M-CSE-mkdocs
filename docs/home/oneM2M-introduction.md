# A Short Introduction to oneM2M

oneM2M is a global standard that defines a *Common Service Layer* for IoT systems. It is designed to be used in IoT scenarios, including smart cities, smart agriculture, smart homes, and many more.

oneM2M is developed by the [oneM2M Partnership Project](https://onem2m.org){target=_new}, which was formed by seven of the world's leading ICT standards development organizations (SDOs) and organizations representing ICT service providers. This partnership project defines and publishes the specifications that form the oneM2M standard.

This article can only provide a short overview about oneM2M. Further introductions and examples can be found at the [oneM2M Recipes site](https://recipes.onem2m.org){target=_new}.

## Common Service Functions

The oneM2M architecture is based on the concept of *Common Service Functions* (CSF) for IoT applications that are provided by a *Common Service Entity* (CSE). A CSE can be implemented in many different ways, for example as a cloud service, on a gateway device, or on a capable IoT device. The *ACME CSE* provides a conformant subset implementation of these common service functions.


<figure markdown="1">
![Figure 2: oneM2M Common Service Functions](../images/oneM2M-CSF.png#only-light){ data-gallery="light"}
![Figure 2: oneM2M Common Service Functions](../images/oneM2M-CSF.png#only-dark){data-gallery="dark"}
<figcaption>Figure 1: oneM2M Common Service Functions (source: oneM2M)</figcaption>
</figure>

As the name suggests, the Common Service Functions provide a useful function set which can be used by an IoT application. An application may use any of the service functions in order to implement its own specific application functionality.
It is important to note that it doesn't matter whether these IoT applications run on a small device, an edge gateway, or in the cloud. But especially for constrained IoT devices it may be important to move some of the IoT-specific application logic to a more capable entity in order to save resources.

Examples for some of the common services are:

* **Data management**: A CSE provides a set of services for storing, retrieving, and managing IoT data. This includes services for storing, retrieving and managing IoT data.
* **Device management**: Another common service function is the management of IoT devices. This includes services for registering IoT devices and managing IoT devices.
* **Security**: An important part is the set of functionalities for securing IoT data and providing access control within a oneM2M system which is provided by a CSE.
* **Communication**: A CSE provides a set of services for communicating within a oneM2M system and with IoT devices. This includes services for sending and receiving IoT data, support for polling as well as for subscribe & notify mechanisms, for managing IoT device communication channels, and protocols.
* **Discovery**: oneM2M provides services for discovery services for IoT devices and IoT data within a oneM2M system. This includes also *Semantic* discovery services. 

and many more.

## Architecture

The following figure shows the basic architecture of oneM2M.

The middle layer (in red) represents the *Common Service Entities* (CSE) and its *Common Service Functions* (CSF). The CSE provides the common services for IoT applications called *Application Entities* (AE, in blue). 

At the bottom the Network Service Entity (NSE, in grey) provides network services for the CSEs. The details of the NSE are not part of the oneM2M standard, but it is an important part of the oneM2M architecture. The NSE provides the connectivity services for the CSEs. This includes services for connecting CSEs and IoT devices to each other, and also provides services to manage network resources and connected devices as well as to provide network security.


<figure markdown="1">
![Figure 2: oneM2M Architecture](../images/oneM2M-Architecture.png#only-light){width=640 data-gallery="light"}
![Figure 2: oneM2M Architecture](../images/oneM2M-Architecture.png#only-dark){width=640 data-gallery="dark"}
<figcaption>Figure 2: oneM2M Architecture (source: oneM2M)</figcaption>
</figure>


### Entities

The following table lists the abbreviations used in the oneM2M architecture.

| Entity                  | Abbreviation | Meaning                                                                                                                                                         |
|-------------------------|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Application Entity      | AE           | Provides application logic for the end‐to‐end M2M solutions.<br /> More generally, this is the IoT Application.                                                 |
| Common Services Entity  | CSE          | Provides the set of “service functions" common to the M2M environments.<br />This is the oneM2M IoT Server.                                                     |
| Network Services Entity | NSE          | Provides connectivity services to the CSEs besides the pure data transport.                                                                                     |
| Node                    |              | Logical equivalent of a physical (or possibly virtualized) device.<br />oneM2M distinguishes between a device/node and the application(s) that run on a device. |


### Reference Points

The oneM2M architecture defines a set of reference points that are used to describe the RESTful interfaces between the different components of a oneM2M system. Figure 2 shows the reference points of the oneM2M architecture. They start with the letters *Mc* and are followed by a letter that indicates the direction of the interface. 

!!! Note
	An application developer will always use the *Mca* reference point to communicate with a CSE. Here, protocols like HTTP, CoAP, MQTT or Websockets can be used to communicate with the CSE and to use the common service functions.

The following table lists the reference points and their meaning.

| Reference Point | Meaning                                                                                                                                    |
|-----------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Mca: CSE - AE   | Interface between a CSE and an Application Entity                                                                                          |
| Mcc: CSE – CSE  | Interface between two CSEs                                                                                                                 |
| Mcn: CSE - NSE  | Interface between a CSE and the Network Service Entity                                                                                     |
| Mcc’            | Interface between two service providers.<br />This reference point is used to connect multiple servide provider domains or oneM2M systems. |

![](https://img.shields.io/github/languages/top/mdjoerg/mqba.svg?style=flat)
![](https://img.shields.io/github/last-commit/mdjoerg/mqba.svg?style=flat)
![](https://img.shields.io/github/issues-raw/mdjoerg/mqba.svg?style=flat)
![](https://img.shields.io/github/languages/code-size/mdjoerg/mqba.svg?style=flat)
![](https://img.shields.io/github/repo-size/mdjoerg/mqba.svg?style=flat)

# What is MQBA?

The MQBA or Message Queue Broker ABAP is an implementation of the MQTT concept for ABAP. This means a simple publish/subscribe mechanism to exchange messages via a central broker instance. See [Wikipedia](https://en.wikipedia.org/wiki/MQTT) for more information about the concept.

The project was created to use the power of the MQTT driven IoT world within SAP ABAP Backend especially for innovation projects.

A bridge to external MQTT brokers (Non-SAP) is included in this project. At the moment this will be done by a gateway service based on [Node-RED](https://nodered.org/).

With ABAP 7.53 SAP includes native MQTT support to the standard kernel. With a release >= 7.53 the project MQBA_PINS can be used.  

# Current State and news

The project is in an beta state. At the moment there are no experiences with productive usage. You can do it at your own risk.  
Some features are planned for the future. So the programs could be changed.
See project news on Twitter with hashtag [#SAPMQBA](https://twitter.com/hashtag/SAPMQBA).

Update 2019/02/17
- native MQTT support with project MQBA_PINS seems to be stable
- german translations added
- german blog [Das SAP ABAP Backend als “IoT Ding”: SAP Monitoring über MQTT](https://joompde.wordpress.com/2019/02/14/das-sap-abap-backend-als-iot-ding-sap-monitoring-uber-mqtt)  

Update 2019/01/04:
- new version with some bugfixes and support for external broker proxies (daemons) added
- Plugin MQBA_PINS with native MQTT protocol support tested for S/4 1809 (ABAP 7.53)
- support for other background broker proxies available


# Technology background

The project uses modern SAP technologies such as ABAP Push Channels (APC), ABAP Messaging Channels (AMC) or Shared Memory to receive messages without polling to save system resources.

Other new(er) technologies are planned for future enhancements.

# Installation and configuration
## ABAP Stack

Install the Project with [ABAPGit](http://abapgit.org).
An SAP Netweaver ABAP Stack 7.50 was used for implementation. A Netweaver 7.40 was not tested so far. The default package for installation is ZMQBA.

Activate the services ZMQBA_GW and ZMQBA_INT (websocket nodes) with transaction SICF if inactive. With newer releases of abapGit this could be done automatically. A system user have to be maintained for the services because external websocket tools do not support user based authorization.

## Bridge to external brokers

If you want to activate the bridge to external MQTT brokers you will need a Node-RED Service and flow as the connector. See [Node-RED-Installation documentation](nodered/README.md) for further information.

The Node-RED Service can be installed on a Raspberry PI or as Docker image for example. Take a look to the [openhabian project](https://github.com/openhab/openhabian). In this distribution for Raspberry PI Node-RED, [Mosquitto MQTT](https://www.mosquitto.org/) and [openHAB](https://www.openhab.org/) are included and preconfigured.

Since ABAP 7.53 a native ABAP MQTT support is existing. If you have an SAP ABAP Netweaver Stack >= 7.53 you can install the MQBA Plugin project [MQBA_PINS](https://github.com/MDJoerg/MQBA_PINS) and can use this implementation instead of the Node-RED gateway. Configure an external broker like iot.eclipse.org (area menu ZMQBA, configuration) and catch the messages via an ABAP daemon service.   

## Customizing

The area menu ZMQBA contains the main transactions and customizing tables.
Enter ZMQBA as transaction code to show this menu.

# Simple Tests and main use cases
## Use internal messaging

Use the following transaction to test the internal messaging:
- ZMQBA_MSG_LIST - display broker messages from memory
- ZMQBA_PUB      - publish a message
- ZMQBA_SUB      - subscribe to a message

Direct private messaging is possible with the session id.

## Use external messaging

External messaging is available through a configured Node-RED-Service.

Use the transaction ZMQBA_PUB with activated flag "as external message". Then the message will be routed to the external gateway. You can see the message with MQTT Clients (e.g. [MQTT.fx](http://www.mqttfx.org/)).

If you want to send a MQTT message from the outside use a MQTT Client and display the broker memory with transaction ZMQBA_MSG_LIST.

## Use programming interface

See function group ZMQBA_API_BROKER for use the broker features from other ABAP programs. A object oriented library is working behind these function modules.

## Realtime messaging to WebUI and Websocket Apps

The internal messaging uses the SICF Service ZMQBA_INT with ABAP Push Channel (APC) Technology and the PCP Protocol. You can connect to websocket ws://yoursaphost:yourport/sap/bc/apc/sap/zmqba_int with a Websocket client like the Chrome plugin [Simple Websocket Client](https://chrome.google.com/webstore/search/simple%20websocket%20client). Or use the SAP APC test application http://yoursaphost:yourport/sap/bc/webdynpro/sap/wdr_test_apc_wsp?apc_application=zmqba_int to receive and send messages.

SAP supports the PCP Protocol with a special javascript library. See [SAP Help](https://help.sap.com/doc/abapdocu_752_index_htm/7.52/en-US/abenpcp.htm) for more information.

# Project supporters

Some organizations and individuals support this project and make it possible. Thank you to:
- [BA GmbH](https://www.ba-gmbh.com/)
- [SAP UCC](http://www.sap-ucc.com/)
- [Lars Hvam](https://github.com/larshp)
- [Gregor Wolf](https://twitter.com/wolf_gregor)

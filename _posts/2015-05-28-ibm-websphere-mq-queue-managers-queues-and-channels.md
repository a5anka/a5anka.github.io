---
layout: post
title: "IBM WebSphere MQ Queue Managers, Queues and Channels"
description: "This post include a summary on IBM WebSphere MQ  Queue Managers, Queues and Channels"
tags: [MQ, IBM, Queue]
image:
  feature: abstract-7.jpg
  credit: dargadgetz
  creditlink: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
comments: true
share: true
---

<figure>
	<a href="{{ site.url }}/images/IBM_MQ_Ecplorer.png">
        <img src="{{ site.url }}/images/IBM_MQ_Ecplorer.png" alt="IBM WebSphere MQ Explorer">
    </a>
</figure>


This post include a summary on IBM WebSphere MQ  Queue Managers, Queues and Channels. Knowing these concepts will help you to do simple tasks like creating a queue, sending and receiving messages from a queue.

## Queue Managers

Queue manager is the top level object that holds in the network (Such as queues and channels). The main tasks done through a queue manager are,

* Start channels
* Process MQI calls
* Create, delete, alter queues and channel definitions
* Run a command server to process MQSC commands

See [IBM Documentation][1] for more details.

## Queues

Queue holds the messages destined to it and send the messages to consumers in the receiving order. A queue has two defined limits.

* Maximum number of messages that it can hold
* Maximum length of a message.

There are several queue types that can be used with the IBM MQ. They are,

* Local queue
* Transmission queue
* Remote queue definition
* Alias queue
* Model queue
* Cluster queue
* Shared queue
* Group definition queue 	

See [IBM Documentation][2] for more details.

## Channels

Channels are used to send and receive messages to and from a queue.

### MQI Channel
Client applications use these channel to send and receive messages. 

* Server connection - This is the server end of the channel
* Client connection

### Message channel
These are used to link two queue managers

See [IBM Documentation][3] for more details.

<!-- References -->
[1]: http://www-01.ibm.com/support/knowledgecenter/SSFKSJ_7.5.0/com.ibm.mq.explorer.doc/e_qmanagers.htm
[2]: http://www-01.ibm.com/support/knowledgecenter/SSFKSJ_7.5.0/com.ibm.mq.explorer.doc/e_queues.htm
[3]: http://www-01.ibm.com/support/knowledgecenter/SSFKSJ_7.5.0/com.ibm.mq.explorer.doc/e_channels.htm

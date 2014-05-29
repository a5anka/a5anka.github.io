---
layout: post
title: "AMQP Publisher-Subscriber example with WSO2 MB server"
description: "this will help you get started on APMQ with WSO2 MB."
tags: [java, amqp, wso2, mb]
image:
  feature: abstract-1.jpg
  credit: dargadgetz
  creditlink: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
comments: true
share: true
---

Have you used the WSO2 MB sever with AMPQ? If not this will help you
get started on APMQ with WSO2 MB. I am using the newly released WSO2
MB 2.2.0 and RabbitMQ AMQP Java client in this example.

## Setting up the project

For this example, I created a Maven project. Following code correspond
to the pom.xml used in the project

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>io.github.a5anka.amqp.helloworld</groupId>
    <artifactId>pub-sub</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>com.rabbitmq</groupId>
            <artifactId>amqp-client</artifactId>
            <version>3.3.1</version>
        </dependency>
    </dependencies>
    
</project>
{% endhighlight %}

We have added RabbitMQ AMQP client as a dependency in the pom.xml
file.

## Publisher

Following Publisher class connect to the WSO2 MB and publish a simple
text message in the message queue "hello".

{% highlight java %}
package io.github.a5anka.amqp.helloworld;

import com.rabbitmq.client.ConnectionFactory;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.Channel;

public class Publisher {

    private final static String QUEUE_NAME = "hello";

    public static void main(String[] argv)
            throws java.io.IOException, InterruptedException {
        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost("localhost");
        factory.setPort(5672);
        factory.setUsername("admin");
        factory.setPassword("admin");
        Connection connection = factory.newConnection();

        Channel channel = connection.createChannel();

        channel.queueDeclare(QUEUE_NAME, true, false, false, null);

        String message = "Hello world!!";
        channel.basicPublish("", QUEUE_NAME, null, message.getBytes());
        System.out.println(" [x] Sent '" + message + "'");

        channel.close();
        connection.close();

    }
}
{% endhighlight %}


## Subscriber

Subscriber code specified below will connect to the WSO2 MB and
retrieve the message published in "hello: message queue and print it
in the console.

{% highlight java %}
package io.github.a5anka.amqp.helloworld;

import com.rabbitmq.client.ConnectionFactory;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.QueueingConsumer;

public class Subscriber {

    private final static String QUEUE_NAME = "hello11";

    public static void main(String[] argv)
            throws java.io.IOException,
            java.lang.InterruptedException {

        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost("localhost");
        factory.setPort(5672);
        factory.setUsername("admin");
        factory.setPassword("admin");
        Connection connection = factory.newConnection();
        Channel channel = connection.createChannel();
        System.out.println(channel);


        channel.queueDeclare(QUEUE_NAME, false, false, false, null);
        System.out.println(" [*] Waiting for messages.");

        QueueingConsumer consumer = new QueueingConsumer(channel);
        channel.basicConsume(QUEUE_NAME, false, consumer);

        while (true) {
            QueueingConsumer.Delivery delivery = consumer.nextDelivery();
            String message = new String(delivery.getBody());
            System.out.println(" [x] Received '" + message + "'");
        }


    }
}
{% endhighlight %}

## Running the example

I assume you have installed the WSO2 MB in your machine and it is run
using the default configuration. You can first run the publisher
code. This will publish a message in to the "hello" queue. Then you
can use subscriber code to retrieve and print the message in the
message queue.

---
layout: post
title: "AMQP Publisher-Subscriber example with WSO2 MB server"
modified: 2015-10-13 21:25:00 +0530
description: "This post will help you get started on AMPQ with WSO2 MB."
tags: [java, amqp, wso2, mb]
image:
  feature: abstract-1.jpg
  credit: dargadgetz
  creditlink: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
comments: true
share: true
---

Have you used the WSO2 MB with AMQP? If not this will help you get
started on AMPQ with WSO2 MB. I am using the newly released
[WSO2MB 3.0.0-Alpha4][1] and RabbitMQ AMQP Java client in this
example.

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

I have added RabbitMQ AMQP client as a dependency in the pom.xml
file.

## Publisher

Publisher class connect to the WSO2 MB and publish a simple
text message to the message queue "hello".

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


## Subscriber - Synchronous

Subscriber code specified below will connect to the WSO2 MB and
retrieve the message published in "hello" message queue and print it
in the console.

{% highlight java %}
package io.github.a5anka.amqp.helloworld;

import com.rabbitmq.client.ConnectionFactory;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.QueueingConsumer;

public class Subscriber {

    private final static String QUEUE_NAME = "hello";

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

        channel.queueDeclare(QUEUE_NAME, true, false, false, null);
        channel.queueBind(QUEUE_NAME, "amq.direct", QUEUE_NAME);
        System.out.println(" [*] Waiting for messages.");

        QueueingConsumer consumer = new QueueingConsumer(channel);
        channel.basicConsume(QUEUE_NAME, false, consumer);

        while (true) {
            QueueingConsumer.Delivery delivery = consumer.nextDelivery();
            String message = new String(delivery.getBody());
            System.out.println(" [x] Received '" + message + "'");

            channel.basicAck(delivery.getEnvelope().getDeliveryTag(), false);
        }
    }
}
{% endhighlight %}

## Subscriber - Asynchronous

Following Subscription implementation receives messages asynchronously.

{% highlight java %}
package io.github.a5anka.amqp.helloworld;

import com.rabbitmq.client.AMQP;
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;
import com.rabbitmq.client.Consumer;
import com.rabbitmq.client.DefaultConsumer;
import com.rabbitmq.client.Envelope;

public class AsyncSubscriber {

    private final static String QUEUE_NAME = "hello";

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

        channel.queueDeclare(QUEUE_NAME, true, false, false, null);
        channel.queueBind(QUEUE_NAME, "amq.direct", QUEUE_NAME);
        System.out.println(" [*] Waiting for messages.");

        Consumer consumer = new DefaultConsumer(channel) {
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope,
                                       AMQP.BasicProperties properties, byte[] body) throws IOException {
                String message = new String(body, "UTF-8");
                System.out.println(" [x] Received '" + message + "'");

                channel.basicAck(envelope.getDeliveryTag(),false);
            }
        };
        channel.basicConsume(QUEUE_NAME, false, consumer);

    }
}
{% endhighlight %}

## Running the example

I assume you have installed the WSO2 MB in your machine and it is run
using the default configuration. You can first run the publisher
code. This will publish a message in to the "hello" queue. Then you
can use subscriber code to retrieve and print the message in the
message queue.

[1]: https://github.com/wso2/product-mb/releases/tag/v3.0.0-ALPHA4

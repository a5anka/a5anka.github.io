---
layout: post
title: "Running Clustered Integration Tests in WSO2 MB"
description: "This post tell you how to configure and run the clustered integration tests introduced in the WSO2 MB 3.0.0."
modified: 2015-11-10
tags: [WSO2, WSO2 MB, Testing]
comments: true
share: true
---

>  “If you don’t like testing your product, most likely your customers won’t like to test it either.” - Anonymous

A message broker or any other software should always be tested
extensively to make sure it is working as expected. That is why we
always try to improve the way we are testing WSO2 MB. Since WSO2 MB is
used mostly in clustered mode, we need to test and see how WSO2 MB
behave in a clustered environment. Prior to WSO2 MB 3.0.0, this was a
manual process which resulted in wasting valuable time and
resources. I am writing this post to tell you how to configure and run
the clustered integration tests introduced in WSO2 MB 3.0.0.

We have not still automated the MB cluster creation process yet.  In a
future release we will be able to automate this process also. But for
the moment we have to create the cluster manually. With the removal of
[Apache ZooKeeper][1] dependency from WSO2 MB, this is not very hard
to do. I am not going to describe how to setup a MB cluster in this
post. You can follow the WSO2 MB documentation to properly setup a MB
cluster.

## Configuring test framework
I am using a two node WSO2 MB cluster for this example. Let's assume
the MB Node details in the cluster are as follows,

#### MB Node 1
* Host name/IP: 192.168.1.5
* HTTP port   : 9763
* HTTPS port  : 9443
* AMQP port   : 5672
* MQTT Port   : 1883

#### MB Node 2
* Host name/IP: 192.168.1.6
* HTTP port   : 9763
* HTTPS port  : 9443
* AMQP port   : 5672
* MQTT Port   : 1883

**1\.** First We have to enter this node information in the [automation.xml][2] file of
the tests-clustering module. An example configuration is mentioned
below

{% highlight xml %}
<!--
    This section will initiate the initial deployment of the platform required by
    the test suites.
-->
<platform>
  <!--
      cluster instance details to be used to platform test execution
  -->
  <productGroup name="MB_Cluster" clusteringEnabled="false" default="true">
    <instance name="mb002" type="standalone" nonBlockingTransportEnabled="false">
      <hosts>
        <host type="default">192.168.1.5</host>
      </hosts>
      <ports>
        <port type="http">9763</port>
        <port type="https">9443</port>
        <port type="qpid">5672</port>
        <port type="mqtt">1883</port>
      </ports>
      <properties>
      </properties>
    </instance>
    <instance name="mb003" type="standalone" nonBlockingTransportEnabled="false">
      <hosts>
        <host type="default">192.168.1.5</host>
      </hosts>
      <ports>
        <port type="http">9763</port>
        <port type="https">9443</port>
        <port type="qpid">5672</port>
        <port type="mqtt">1883</port>
      </ports>
      <properties>
      </properties>
    </instance>
  </productGroup>
</platform>
{% endhighlight %}


**2.** Also edit the datasource information in the [automation.xml][2]
  to match your MySQL database configuration for the message
  store. Please note that we currently only support MySQL database for
  Cluster Testing even-though WSO2 MB supports several database systems
  like MySQL, MSSQL, and Oracle.

{% highlight xml %}
<!--
Database configuration to be used for data service testing. DB configuration in dbs files will be replaced with
below configuration at test run time
-->
<datasources>
  <datasource name="mbCluster">
    <url>jdbc:mysql://localhost/WSO2_MB</url>
    <username>root</username>
    <password>root</password>
    <driverClassName>com.mysql.jdbc.Driver</driverClassName>
  </datasource>
</datasources>
{% endhighlight %}

**3\.** We have to also change the value of "skipPlatformTests" property to
false in the
[pom.xml](https://github.com/wso2/product-mb/blob/master/modules/integration/tests-platform/tests-clustering/pom.xml)
of tests-clustering module to enable clustered integrations tests.

{% highlight xml %}
<properties>
  <skipPlatformTests>false</skipPlatformTests>
</properties>
{% endhighlight %}

**4\.**  Now we can run the clustered test by executing `mvn clean install` in the
product-mb/modules/integration/ directory.

<figure>
	<a href="{{ site.url }}/images/source_control_angriestprogrammer_com.png">
        <img src="{{ site.url }}/images/Testing_andy_glover.jpg" alt="Source Control">
    </a>
</figure>

[1]: https://zookeeper.apache.org/
[2]: https://github.com/wso2/product-mb/blob/master/modules/integration/tests-platform/tests-clustering/src/test/resources/automation.xml

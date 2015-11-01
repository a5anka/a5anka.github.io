---
layout: post
title: "Running Clustered Integration Tests in WSO2 MB"
description: "Clustered integration tests are introduced in WSO2 MB 3.0.0. Before
version 3.0.0. This post tell you how to configure and run the clustered integration tests available in the WSO2 MB."
tags: [WSO2, WSO2 MB, Testing]
comments: true
share: true
---

Clustered integration tests are introduced in WSO2 MB 3.0.0. Before
version 3.0.0 we did not have clustered integration tests. Therefore
testing MB in a cluster was a manual process. This resulted in wasting
valuable time and resources.  I am writing this post to tell you how
to configure and run the clustered integration tests available in the
WSO2 MB.

Still the MB cluster creation process has to be done manually. In a
future release we might be able to automate this process also. But for
the moment we have to create cluster manually. With the removal of
zoo-keeper server requirement, this is not very hard to do. First we
need to setup the Cassandra ring properly. Then configure MB to use
the Cassandra ring and configure MB properly in cluster mode. I am not
going to describe how to setup a MB cluster in this post.

## Configuring test framework
Let's assume the MB Node details in the cluster are as follows,

#### MB Node 1
* Host name/IP: 192.168.1.5
* HTTP port   : 9763
* HTTPS port  : 9443
* AMQP port   : 5672

#### MB Node 2
* Host name/IP: 192.168.1.6
* HTTP port   : 9763
* HTTPS port  : 9443
* AMQP port   : 5672


We have to enter this node information in the [automation.xml](https://github.com/wso2-dev/product-mb/blob/master/modules/integration/tests-platform/tests-clustering/src/test/resources/automation.xml) file of
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
      </ports>
      <properties>
      </properties>
    </instance>
  </productGroup>
</platform>
{% endhighlight %}


We have to also change the value of "skipPlatformTests" property to
false in the
[pom.xml](https://github.com/wso2-dev/product-mb/blob/master/modules/integration/tests-platform/tests-clustering/pom.xml)
of tests-clustering module to enable clustered integrations tests.

{% highlight xml %}
<properties>
  <skipPlatformTests>false</skipPlatformTests>
</properties>
{% endhighlight %}

Now we can run the clustered test by executing `mvn clean install` in the
product-mb repository.

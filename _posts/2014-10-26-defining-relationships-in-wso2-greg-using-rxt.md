---
layout: post
title: "Defining Relationships in WSO2 GREG using RXT"
description: "This post explain the relationship model used in the WSO2 GREG's RXT files"
tags: [WSO2, GREG, RXT]
image:
  feature: abstract-3.jpg
  credit: dargadgetz
  creditlink: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
comments: true
share: true
---

WSO2 GREG uses RXT files to describe the data model of an
artifact. There are mainly two steps in creating an artifact using RXT
files.

1. Creating the RXT file. You can find details about the syntax used in RXT files in [here](https://docs.wso2.com/display/Governance460/Governance+Artifacts+Configuration+Model+Elements).
2. Deploying created RXT file in GREG. This is described in detail in [here](https://docs.wso2.com/display/Governance460/Deploying+an+Extension+File).

## Relationship Model

Following is an explanation about the relationship model used in the
GREG's RXT files. In RXT files, we use the "relationships element"
([more details here](https://docs.wso2.com/display/Governance460/Governance+Artifacts+Configuration+Model+Elements#GovernanceArtifactsConfigurationModelElements-TherelationshipsElement))
to define relationships between two artificats. You can find several
RXT examples inside `$GREG_HOME/samples/asset- models`
directory. Following is an extract from the sample file
`$GREG_HOME/samples/asset-models/TestPlanModel/registry-extensions/TestSuite.rxt`.

{% highlight xml linenos %}
<relationships>
  <association type="usedBy" source="@{testCases_entry:value}"/>
  <dependency type="depends" target="@{testCases_entry:value}"/>
</relationships>
{% endhighlight %}

Here in line 2, we have added a "usedBy" association in the TestCase
artifact to the TestSuite artifact using the association element. Note
that we have used source attribute to indicate that the relationship
is from "TestCase" to "TestSuite". In the line 3 we have added a
dependency in the TestSuite on "TestCase". Note that we have used the
"target" attribute to indicate that the relationship is from
"TestSuite" to "TestCase". The relationships element only indicate the
type of relationship. To make it relate to one or many, we can use
different type of fields. You can find a description about available
field types from [here](https://docs.wso2.com/display/Governance460/Governance+Artifacts+Configuration+Model+Elements#GovernanceArtifactsConfigurationModelElements-ThefieldElement).

If we look at the same sample file we took earlier we can see that a
"TestSuite" artifact have an association with many "TestCase"
artifacts. The relevant RXT code is mentioned below.

{% highlight xml linenos %}
<table name="TestCases">
  <subheading>
    <heading>Type</heading>
    <heading>Path</heading>
  </subheading>
  <field type="option-text" maxoccurs="unbounded" path="true">
    <name>Case</name>
    <values>
      <value>Test Case</value>
    </values>
  </field>
</table>
{% endhighlight %}

In line 6 we have defined the field type as "option-text" with
attribute "maxoccurs" set to "unbounded". Therefore we can add many
TestCases for a Testsuite. If we have used a field of type text field
then a TestSuite can have only one TestCase.

Similarly you can use different combination of the "relationships"
elements and field elements to create different relationship patterns.

## Examples
Some Examples are listed below, I am taking "Project group" and
"Person" as artifcats for following examples. I have only shown the
relevant parts of the RXT file. You have to add all other required
elements to make it a valid RXT file.

### One-to-one

{% highlight xml %}
<!-- In Person RXT definition -->
<relationships>
    <association type="groupOwnerOf" source="@{overview_group}"/>
    <association type="own" target="@{overview_group}"/>
</relationships>

<content>
    <table name="Overview">
        <field type="text" required="true" path="true">
            <name>Group</name>
        </field>
    </table>
</content>
{% endhighlight %}

### Many-to-many

{% highlight xml %}
<!-- In Person RXT definition -->
<relationships>
    <association type="member" source="@{membership_entry:value}"/>
    <association type="memberOf" target="@{membership_entry:value}"/>
</relationships>

<content>
    <table name="Membership">
        <subheading>
            <heading>Type</heading>
            <heading>Path</heading>
        </subheading>
        <field type="option-text" maxoccurs="unbounded" path="true">
            <name>Member</name>
            <values>
                <value>Junior</value>
                <value>Senior</value>
            </values>
        </field>
    </table>
</content>
{% endhighlight %}

### One-to-many

{% highlight xml %}
<!-- In ProjectGroup RXT -->
<relationships>
    <association type="member" target="@{members_entry:value}"/>
</relationships>

<content>
    <table name="Members">
        <subheading>
            <heading>Type</heading>
            <heading>Path</heading>
        </subheading>
        <field type="option-text" maxoccurs="unbounded" path="true">
            <name>Member</name>
            <values>
                <value>Junior</value>
                <value>Senior</value>
            </values>
        </field>
    </table>
</content>
{% endhighlight %}

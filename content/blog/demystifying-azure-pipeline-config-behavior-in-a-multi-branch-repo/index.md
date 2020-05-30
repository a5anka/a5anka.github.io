---
title: "Demystifying Azure Pipeline Config Behavior"
description: "How I understood the behavior of an Azure Pipeline Config in a multi branch repo"
date: "2020-05-08"
---

Every serious software project uses an automation server or a service
to implement CI/CD pipelines to ensure that the changes are pushed to
production with good quality in a consistent manner. Azure pipelines
is such a popular cloud service to implement CI/CD pipelines. There
are a couple of ways to define a pipeline in Azure. The visual user
interface is one where a user has to go through a series of
configuration pages. The other approach is defining the pipeline in a
YAML file which is stored along with the source code.

![Azure Pipeline Configuration](pipeline-config.png
"https://devblogs.microsoft.com/devops/announcing-general-availability-of-azure-pipelines-yaml-cd/")

I prefer using version controlled YAML configs over the GUI based
configuration since we can apply all the Git and GitHub related magic
to pipeline configs. Microsoft recently [announced the general
availability of YAML CD features in Azure
Pipelines](https://devblogs.microsoft.com/devops/announcing-general-availability-of-azure-pipelines-yaml-cd/).
The YAML based approach and the GUI based approach are very similar if
we compare the features. But there is one big difference in the YAML
based config due to the nature of Git. When we create the pipeline
using the graphical interface, probably the pipeline configuration is
stored in persistence storage like a database. Therefore there can
be always one copy of the pipeline configuration. But when we use a
YAML file stored in Git to define a pipeline, there can be multiple
versions of the pipeline config in different Git branches.

I wanted to understand how Azure Pipeline service decides which config
to pick when there are multiple branches. Therefore I conducted a
small experiment. I started from the [sample
repository](https://github.com/MicrosoftDocs/pipelines-java) provided
by Azure Pipelines for Java applications. I created two branches in
the repo called `master` and `foo`. Then I created an Azure Pipeline
and used `azure-pipelines.yml` as the config file of the pipeline.
This file exists in both branches. You might already know that the
`trigger` parameter is used in the pipeline config file to specify the
branch that should be watched by a pipeline for changes. I tried all
combinations of this parameter in two branches while pushing the
changes to braches. The following is what I got.

![Summary of results](result-summary.png "Summary of results" )

It is evident from the results that the logic used in the Azure
Pipeline implementation is not a very complex one. We can see that
every time the pipeline has triggered the configuration in the updated
file's branch is pointing to itself. i.e. when a file is updated in
the `foo` branch the pipeline is triggered every time the
`azure-pipelines.yml` config in the `foo` branch has the `foo` branch
as the `trigger`.

![Analysis of results](result-summary-circled.png "Analysis of
results" )

In the [next post](../common-azure-pipeline-automations-part-1), let's have a look at a few use cases
and see how to apply this knowledge.


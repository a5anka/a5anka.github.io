---
title: "Common Azure pipeline trigger use cases - part 1"
description: "Examines how the trigger paramter can be used to implement some of the common automations"
date: "2020-05-30"
---

Azure pipelines are also kind of programs. There are inputs, outputs and a processing logic. In most
cases the input is a source repository. Output is the release artifacts like binaries. The
processing logic is written using YAML adhering to the schema defined by Azure pipelines. Like all
the programs, some event should trigger the execution of the program. These events are defined by
the triggers configured in a pipeline. There are mainly two trigger types configurable in a Azure
pipeline YAML config. They are Continuous integration (CI) triggers and Pull request validation (PR)
triggers.

CI triggers and PR triggers depends on the repository used with the pipeline. Adding commits to a
branch and creating a tag are two examples for CI triggers. Opening a PR and pushing changes to a PR
are two examples for PR triggers. You can learn more about this different type of triggers from
[official
documentation](https://docs.microsoft.com/en-us/azure/devops/pipelines/build/triggers?view=azure-devops).
Here is an example showcasing the different config options available with triggers. 

```yaml
trigger:
  branches:
    include:
    - master
    - releases/*
    exclude:
    - releases/old*
  tags:
    include:
    - v2.*
    exclude:
    - v2.0
  paths:
    include:
    - docs/*
    exclude:
    - docs/README.md

# Pull request related triggers
pr:
  branches:
    include:
    - master
    - releases/*
  paths:
    include:
    - docs/*
    exclude:
    - docs/README.md
```

I think the config options are self explanatory and needs no detail explanations. Let's look at some
common usecase and see how pipeline triggers are configured to implement them in these series of
posts. We will start with the most basic one.

### Running Tests and build for all changes pushed

Maintaining quality in a source repository is vital for both the user and developer productivity.
The most fundamental way to do that is by keeping the build passing with tests. All the serious
software projects use some sort of a build tool to do this. Therefore it's just a matter of running
the build tool against the current version of the source code to see whether it is broken or not. It
is a common practice to use a build automation tool like [Azure
Pipelines](https://azure.microsoft.com/en-us/services/devops/pipelines/), [GitHub
Actions](https://github.com/features/actions), [Jenkins](https://www.jenkins.io/) to perform the
build so that the maintainers can take actions on build breaks.

The expected functionality from the build automation tool for this use case is very simple. i.e.
watch for code pushes to all branches in the repository an trigger a build. In a repository, there
is the default branch (normally called master) which hosts the latest working version of the
project. Then there will be few feature branches where the active feature development is done. There
can also be few release branches used to release patch versions, etc.

Configuring build automation for all branches of a source repository in Azure Pipeline is very
trivial: Have nothing in the trigger sections. Yes, you read it correctly. When a trigger is not
specified in the pipeline, the default implicit trigger is following.

```yaml
trigger:
  branches:
    include:
	- '*'
```
If you don't like having no triggers in the pipeline config, you can have above to make it explicit.

The build pipeline can be configured to build all branches except certain branches by using the
`triggger/branches/exclude` config. I would recommend against it since it is always better to get
the build status specially when it is the broken state.
 
...

This is a very trivial trigger use case that can be achieved with Azure pipelines. We will look at
more trigger use cases (pushing building and pushing artifacts, running checks against a created PR,
updating a deployment with changes, etc) in the next few posts.

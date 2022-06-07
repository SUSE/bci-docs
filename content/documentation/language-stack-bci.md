---
title: Language stack SLE Base Container Images
slug: language-stack-bci
---

If you have a working knowledge of containers, you will not have any
difficulties using SLE BCIs. However, there are certain features that set SLE
BCIs apart from similar offerings, like images based on Debian or Alpine
Linux. And understanding the specifics can help you to get the most out of SLE
BCIs in the shortest time possible.

## Language stack SLE BCIs

Language stack SLE BCI are built on top of BCI-Base. Below is an overview of the
available language stack SLE BCIs.

### python

* URL: [`registry.suse.com/bci/python`](https://registry.suse.com/static/bci/python/index.html)

* Tags: `3.6`, `3.9`, `3.10`

* Ships with the python3 version from the tag and pip3, curl, git tools.

### node

* URL: [`registry.suse.com/bci/node`]()

* Tags: `12`, `14`, `16`

* Comes with nodejs version from the tag, npm and git. The yarn package manager
  can be installed with the `npm install -g yarn` command.

### openjdk

* URL: [`registry.suse.com/bci/openjdk`](https://registry.suse.com/static/bci/openjdk/index.html)

* Tags: `11`, `17`

* Ships with the OpenJDK runtime. Designed for deploying Java applications.

### openjdk-devel

* URL: [`registry.suse.com/bci/openjdk-devel`](https://registry.suse.com/static/bci/openjdk-devel/index.html)

* Tags: `11`, `17`

* Includes the development part of OpenJDK in addition to the OpenJDK
  runtime. Instead of Bash, the default entry point is the jshell shell.

### ruby

* URL: [`registry.suse.com/bci/ruby`](https://registry.suse.com/static/bci/ruby/index.html)

* Tags: `2.5`

* A standard development environment based on Ruby 2.5, featuring ruby, gem and
  bundler as well as git and curl.

### golang

* URL: [`registry.suse.com/bci/golang`](https://registry.suse.com/static/bci/golang/index.html)

* Tags: `1.16`, `1.17`, `1.18`

* Ships with the go compiler version specified in the tag.

### dotnet-runtime

* URL: [`registry.suse.com/bci/dotnet-runtime`](https://registry.suse.com/static/bci/dotnet-runtime/index.html)

* Tags: `3.1`, `5.0`, `6.0`

* Includes the .NET runtime from Microsoft and the Microsoft .NET repository.

### dotnet-aspnet

* URL: [`registry.suse.com/bci/dotnet-aspnet`](https://registry.suse.com/static/bci/dotnet-aspnet/index.html)

* Tags: `3.1`, `5.0`, `6.0`

* Ships with the ASP.NET runtime from Microsoft and the Microsoft .NET
  repository.

### dotnet-sdk

* URL: [`registry.suse.com/suse/dotnet-sdk`](https://registry.suse.com/static/bci/dotnet-sdk/index.html)

* Tags: `3.1`, `5.0`, `6.0`

* Comes with the .NET and ASP.NET SDK from Microsoft as well as the Microsoft
  .NET repository.

### rust

* URL: [`registry.suse.com/bci/rust`](https://registry.suse.com/static/bci/rust-beta/index.html)

* Tags: `f1.56`, `1.57`, `1.58`, `1.59`

* Ships with the Rust compiler and the cargo package manager.

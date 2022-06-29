---
Title: How To Use BCIs As VScode Development Containers
slug: vscode-dev-containers
---

Visual Studio Code has a feature called [Development Containers](https://code.visualstudio.com/docs/remote/create-dev-container).
This is part of the built-in functionality to work with remote containers. The
SLE BCI language stack containers make a great choice to use as a development
environment.

## Why Use The SLE BCI In Development Containers

There are two reasons the BCI language stack containers are useful for development.

First, you develop in the same environment as when you use a BCI language stack
image to build or run your application. Being able to develop and build or run
your application in the same environment setup enables you to discover quirks
and issues that are related to the way your application works in the environment.

Second, when you have a team of people working on an application, everyone
developing the application can have the same environment to work in. Whether they
are on Windows, macOS, or Linux the environment the environment will be the same.

## Docker Socket Required

VS Code creates the containers to do development in by using Docker Engine and
it communicates over the Docker socket. That means you need a Docker socket
available on your system.

[Rancher Desktop](https://rancherdesktop.io) is our recommended app for working
with containers. It is available for Windows, macOS, and Linux. Alternatively,
you can use another tool such as Docker Desktop or installing Docker Engine
directly on your system if you are using Linux.

{{< hint type=note >}}
VS Code mounts your code from the local system inside the container. It works
best when the container runtime is on your local system as opposed to remote.
While possible to do this with the container runtime on a remote system, this
guide does not cover handling that situation. {{< /hint >}}

## Dev Container Basics

Dev containers enable you to mount any folder inside a container where you can
specify the environment. Debugging, extensions, and other features work with
the code as it is mounted within the container rather than the location on the
local file system.

## Dev Container Configuration

Configuration for Development Containers is stored in a file named
`.devcontainer/devcontainer.json`. When you open up a codebase with this
configuration file present, VS Code will read the configuration and present you
with an option to use a Dev Container for development.

There are two ways to specify where to get the container from. You can point it
at an image or you can point it at a _Dockerfile_ to build the image from. Since
VS Code needs some additions installed you can't simply point it at a BCI image.

To illustrate using the `Dockerfile` method we can look at a setup for the Go
programming language. A `Dockerfile` placed in the `.devcontainer` directory
would look like:

```dockerfile
FROM registry.suse.com/bci/golang:1.18

# Install tools needed by Visual Studio Code Remote Development Containers
RUN zypper --non-interactive install -y tar git gzip
```

VS Code needs git, gzip, and tar installed which are not present in the Go BCI
image by default. They need to be installed.

While this example is targeted at Go, it will work for other languages where
there is a [BCI language stack]({{< relref "/documentation/language-stack-bci.md" >}})
available.

This file needs to be referenced in the `devcontainer/devcontainer.json` file.
For example, this file could look like:

```json
{
	"name": "Golang",
	"build": {
		"dockerfile": "Dockerfile"
	}
}
```

If you open up a project with these files in them VS Code will prompt you to open
them in a container.

This example JSON configuration file is in its simplest form. You can learn more
details about the additional configuration in the
[VS Code documentation for Development Containers](https://code.visualstudio.com/docs/remote/create-dev-container).

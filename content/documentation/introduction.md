---
title: Introduction to SLE Base Container Images
slug: introduction
---

SLE Base Container Image (SLE BCI) are minimal SUSE Linux Enterprise Server
15-based images that you can use to develop, deploy, and share
applications. There are two types of SLE BCI:

* General-purpose SLE BCI can be used for building custom container images and
  for deploying applications.
* Language stack SLE BCI provide minimal environments for developing and
  deploying applications in specific programming languages.

In addition to that, we will provide Application Container Images based on SLE
BCI featuring popular containerized applications like Nginx, PostgreSQL, MariaDB
and [RMT](https://github.com/SUSE/rmt).

## Highlights

* SLE BCI are fully compatible with SUSE Linux Enterprise Server, but they do
  not require a subscription to run and distribute them.

* SLE BCI automatically run in FIPS-compatible mode when the host operating
  system is running in FIPS mode.

* Each SLE BCI includes the RPM database, which makes it possible to audit the
  contents of the container image. You can use the RPM database to determine the
  specific version of the RPM package any given file belongs to. This allows you
  to ensure that a container image is not susceptible to known and already fixed
  vulnerabilities.

* All SLE BCI (except for those without `zypper`) come with the
  `container-suseconnect` service. This gives containers that run on a registered
  SLES host access to the full SLES repositories. `container-suseconnect` is
  invoked automatically when you run `zypper` for the first time, and the service
  adds the correct SLES repositories into the running container. On an
  unregistered SLES host or on a non-SLES host, the service does nothing.

### General-purpose SLE BCI

There are four general purpose SLE BCI, and each container image comes with a
minimum set of packages to keep its size low. You can use a general purpose SLE
BCI either as a starting point for building custom container images, or as a
platform for deploying specific software. For more information about general
purpose SLE BCI, see [here]({{< relref "/documentation/general-purpose-bci.md"
>}}).

### Language stack SLE BCI

Language stack SLE BCI are built on top of the BCI-Base general-purpose SLE
BCI. Each container image comes with the zypper stack and the free `SLE_BCI`
repository. Additionally, each image includes most common tools for building and
deploying applications in the specific language environment. This includes tools
like a compiler or interpreter as well as the language specific package
manager. For more information about language stack SLE BCI, see [here]({{<
relref "/documentation/language-stack-bci.md" >}}).

### Important note on status and lifecycle

All container images, except for `bci-base`, are currently classified as tech
preview, and SUSE doesn't provide official support for them. This information is
visible on the web on [registry.suse.com](https://registry.suse.com). In
addition to that, it is also indicated via the `com.suse.supportlevel` label
whether a container image still has the tech preview status. You can use the
[`skopeo`](https://github.com/containers/skopeo) and
[`jq`](https://stedolan.github.io/jq/) utilities to check the status of the
desired SLE BCI as follows:

```ShellSession
❯ skopeo inspect docker://registry.suse.com/bci/bci-micro:15.4 | jq '.Labels["com.suse.supportlevel"]'
"techpreview"

❯ skopeo inspect docker://registry.suse.com/bci/bci-base:15.4 | jq '.Labels["com.suse.supportlevel"]'
"l3"
```

In the example above, the `com.suse.supportlevel` label is set to `techpreview`
in the `bci-micro` container image, indicating that the image still has the tech
preview status. The `bci-base` container image on the other hand is fully l3
supported. Unlike like the general purpose SLE BCI, the language stack SLE BCI
may not follow the lifecycle of the SLE distribution: they are supported as long
as the respective language stack receives support. In other words, new versions
of SLE BCI (indicated by the OCI tags) may be released during the lifecycle of a
SLE Service Pack, while older versions may become unsupported. Refer to
[suse.com/lifecycle](https://suse.com/lifecycle) to find out whether the
container in question is still under support.


### Getting started

The SLE BCI are available as OCI-compatible container images directly from
registry.suse.com and can be used like any other container image. For example,
using one of the general purpose containers:

{{< tabs "run_sle_bci">}}
{{< tab "Docker">}}
```ShellSession
❯ docker run --rm -it registry.suse.com/bci/bci-base:15.4 grep '^NAME' /etc/os-release
NAME="SLES"
```
{{< /tab >}}
{{< tab "Podman" >}}
```ShellSession
❯ podman run --rm -it registry.suse.com/bci/bci-base:15.4 grep '^NAME' /etc/os-release
NAME="SLES"
```
{{< /tab >}}
{{< tab "nerdctl" >}}
```ShellSession
❯ nerdctl run --rm -it registry.suse.com/bci/bci-base:15.4 grep '^NAME' /etc/os-release
NAME="SLES"
```
{{< /tab >}}
{{< /tabs >}}

Alternatively, you can use SLE BCI in a `Dockerfile` as follows:

```Dockerfile
FROM registry.suse.com/bci/bci-base:15.4
RUN zypper -n in python3 && \
    echo "Hello Green World!" > index.html
ENTRYPOINT ["/usr/bin/python3", "-m", "http.server"]
EXPOSE 8000
```
You can then build container images using your favorite container runtime:

{{< tabs "build_example_dockerfile">}}
{{< tab "Docker">}}
```ShellSession
❯ docker build .
Sending build context to Docker daemon  2.048kB
Step 1/4 : FROM registry.suse.com/bci/bci-base:15.4
 ---> e34487b4c4e1
Step 2/4 : RUN zypper -n in python3 &&     echo "Hello Green World!" > index.html
 ---> Using cache
 ---> 9b527dfa45e8
Step 3/4 : ENTRYPOINT ["/usr/bin/python3", "-m", "http.server"]
 ---> Using cache
 ---> 953080e91e1e
Step 4/4 : EXPOSE 8000
 ---> Using cache
 ---> 48b33ec590a6
Successfully built 48b33ec590a6


❯ docker run -p 8000:8000 --rm -d 48b33ec590a6
575ad7edf43e11c2c9474055f7f6b7a221078739fc8ce5765b0e34a0c899b46a


❯ curl localhost:8000
Hello Green World!
```
{{< /tab >}}
{{< tab "Podman" >}}
```Shell
❯ buildah bud --layers .
STEP 1/4: FROM registry.suse.com/bci/bci-base:15.4
STEP 2/4: RUN zypper -n in python3 &&     echo "Hello Green World!" > index.html
--> Using cache 8541a01ef66f1e43f850d30d756628fe301ae0ffe09dd3918d7e64d6e1788a3a
--> 8541a01ef66
STEP 3/4: ENTRYPOINT ["/usr/bin/python3", "-m", "http.server"]
--> Using cache 61cccdaa38aab5a44b0ef24935f4aa671f3231b611e0fa45c32ce869da6f9461
--> 61cccdaa38a
STEP 4/4: EXPOSE 8000
--> Using cache 3e93a763b2d0a56ffe70429ca05a110288a868b46b92f47c1609a1129d058383
--> 3e93a763b2d
3e93a763b2d0a56ffe70429ca05a110288a868b46b92f47c1609a1129d058383

❯ podman run --rm -d -p 8000:8000 3e93a763b2d0a56ffe70429ca05a110288a868b46b92f47c1609a1129d058383
e6115cbd37cf94781597cb7b8ade500951e7f4206b13102bdd9e603279378e17

❯ curl localhost:8000

Hello Green World!
```
{{< /tab >}}
{{< /tabs >}}

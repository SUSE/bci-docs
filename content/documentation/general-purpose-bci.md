---
title: BCI-Base, BCI-Minimal, BCI-Micro, and BCI-BusyBox
slug: general-purpose-bci
---

SUSE offers several general-purpose SLE Base Container Images that are intended
as deployment targets or as foundations for creating customized images:
BCI-Base, BCI-Minimal, BCI-Micro, and BCI-BusyBox. These images share the common
SLES base, and none of them ship with a specific language or an application
stack. All images feature the RPM database (even if the specific image does not
include the RPM package manager) that can be used to verify the provenance of
every file in the image. Each image includes the SLES certificate bundle, which
allows the deployed applications to use the system's certificates to verify TLS
connections.


## Quick overview

The table below provides a quick overview of the differences between BCI-Base,
BCI-Minimal, BCI-Micro, and BCI-BusyBox.


## BCI-Base and BCI-Init: When you need flexibility

This SLE BCI comes with the Zypper package manager and a free SLE-BCI
repository. This allows you to install software available in the repository and
customize the image during the build. The downside is the size of the image. It
is the largest of the general-purpose SLE BCIs, so it is not always the best
choice for a deployment image.

A variant of BCI-Base called BCI-Init comes with systemd preinstalled. The
BCI-Init container image can be useful in scenarios requiring systemd for
managing services in a single container.

## BCI-Minimal: When you do not need Zypper

This is a stripped-down version of the BCI-Base image. BCI-Minimal comes without
Zypper, but it does have the RPM package manager installed. This significantly
reduces the size of the image. However, while RPM can install and remove
packages, it lacks support for repositories and automated dependency
resolution. The BCI-Minimal image is therefore intended for creating deployment
containers, and then installing the desired RPM packages inside the
containers. Although you can install the required dependencies, you need to
download and resolve them manually. However, this approach is not recommended as
it is prone to errors.

## BCI-Micro: When you need to deploy static binaries

This image is similar to BCI-Minimal but without the RPM package manager. The
primary use case for the image is deploying static binaries produced externally
or during multi-stage builds. As there is no straightforward way to install
additional dependencies inside the container image, we recommend deploying a
project using the BCI-Minimal image only when the final build artifact bundles
all dependencies and has no external runtime requirements (like Python or Ruby).


## BCI-BusyBox: When you need the smallest and GPLv3-free image

Similar to BCI-Micro, the BCI-BusyBox image comes with the most basic tools
only. However, these tools are provided by the BusyBox project. This has the
benefit of further size reduction. Furthermore, the image contains no GPLv3
licensed software. When using the image, keep in mind that there are certain
differences between the BusyBox tools and the GNU Coreutils. So scripts written
for a system that uses GNU Coreutils may require modification to work with
BusyBox.

## Approximate sizes

For your reference, the list below provides an approximate size of each SLE
BCI. Keep in mind that the provided numbers are rough estimations.

- BCI-Base ~94 MB

- BCI-Minimal ~42 MB

- BCI-Micro ~26 MB

- BCI-BusyBox ~14 MB

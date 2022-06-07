---
title: Why SLE Base Container Images
slug: why-sle-bci
---

SLE BCIs offer a platform for creating SLE-based custom container images and
containerized applications that can be distributed freely. SLE BCIs feature the
same predictable enterprise lifecycle as SLES. The `SLE_BCI` 15 SP3 and SP4
repository (which is a subset of the SLE repository) gives SLE BCIs access to
4,000 packages available for the AMD64/Intel 64, AArch64, ppc64le, and s390x
architectures. The packages in the repository have undergone quality assurance
and security audits by SUSE. The container images are FIPS-compliant when
running on a host in FIPS mode. In addition to that, SUSE can provide official
support for SLE BCIs through SUSE subscription plans.

### Security

Each package in the SLE_BCI repository undergoes security audits, and SLE BCIs
benefit from the same mechanism of dealing with CVEs as SLES. All discovered and
fixed vulnerabilities are announced via e-mail, the dedicated [CVE
pages](https://www.suse.com/security/cve/), and as OVAL and CVRF data. To ensure
a secure supply chain, all container images are signed with Notary v1, Podman's
GPG signatures, and Sigstore Cosign.

### Stability

Since SLE BCIs are based on SLE, they feature the same level of stability and
quality assurance as SUSE Linux Enterprise Server. Similar to SLES, SLE BCIs
receive maintenance updates that provide bug fixes, improvements, and security
patches.

### Tooling and integration

SLE BCIs are designed to provide drop-in replacements for popular container
images available on hub.docker.com. You can use the general-purpose SLE BCIs and
the tools they put at your disposal to create custom container images, while the
language stack SLE BCIs provide a foundation and the required tooling for
building containerized applications.

### Redistribution

BCIs are covered by a permissive
[EULA](https://www.suse.com/licensing/eula/#bci) that allows you to redistribute
custom container images based on a BCI.

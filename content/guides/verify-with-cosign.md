---
title: Verify SUSE SLE Base Container Images With Cosign
slug: verify-with-cosign
---

SUSE has SLE Base Container Images (BCI) that are great to use in workflows and
as a based under your applications. One of the big reasons I like these images
is that they are constantly updated with fixes for Common Vulnerabilities and
Exposures (CVE). SUSE stays on top of this and takes security seriously.

For example, prior to writing this post I used Trivy to scan the Go image:

```ShellSession
❯ trivy i registry.suse.com/bci/golang:latest
2022-03-11T09:12:48.961-0500	INFO	Detected OS: suse linux enterprise server
2022-03-11T09:12:48.961-0500	INFO	Detecting SUSE vulnerabilities...
2022-03-11T09:12:48.962-0500	INFO	Number of language-specific files: 21

registry.suse.com/bci/golang:latest (suse linux enterprise server 15.3)
=======================================================================
Total: 0 (UNKNOWN: 0, LOW: 0, MEDIUM: 0, HIGH: 0, CRITICAL: 0)
```

The same can be found for the other images and on the most recent tags.


## How To Verify Images With Cosign

[Cosign](https://github.com/SigStore/cosign) provides the ability to sign and
verify images (and other things). It’s a project from
[Sigstore](https://www.sigstore.dev/), a sub-foundation of the Linux
Foundation. The BCIs can now be verified using Cosign. For example:

{{< tabs "verify_with_cosign">}}
{{< tab "local install">}}
```ShellSession
❯ cosign verify \
    --key https://ftp.suse.com/pub/projects/security/keys/container–key.pem \
    registry.suse.com/bci/bci-base:latest | tail -1 | jq

[
  {
    "critical": {
      "identity": {
        "docker-reference": "registry.suse.com/bci/bci-base"
      },
      "image": {
        "docker-manifest-digest": "sha256:52a828600279746ef669cf02a599660cd53faf4b2430a6b211d593c3add047f5"
      },
      "type": "cosign container image signature"
    },
    "optional": {
      "creator": "OBS"
    }
  }
]
```
{{</ tab >}}
{{< tab "using docker" >}}
```ShellSession
❯ docker run --rm -it gcr.io/projectsigstore/cosign verify \
    --key https://ftp.suse.com/pub/projects/security/keys/container–key.pem \
    registry.suse.com/bci/bci-base:latest | tail -1 | jq

[
  {
    "critical": {
      "identity": {
        "docker-reference": "registry.suse.com/bci/bci-base"
      },
      "image": {
        "docker-manifest-digest": "sha256:52a828600279746ef669cf02a599660cd53faf4b2430a6b211d593c3add047f5"
      },
      "type": "cosign container image signature"
    },
    "optional": {
      "creator": "OBS"
    }
  }
]
```
{{< /tab >}}
{{< /tabs >}}


You need to specify a key because SUSE images are signed with a secured SUSE
key.

If you want to check the images against
[rekor](https://github.com/sigstore/rekor), the immutable tamper resistant
ledger, you can do so. For example:

{{< tabs "check_against_rekor">}}
{{< tab "local install">}}
```ShellSession
❯ COSIGN_EXPERIMENTAL=1 cosign verify \
    --key https://ftp.suse.com/pub/projects/security/keys/container–key.pem \
    registry.suse.com/bci/bci-base:latest | tail -1 | jq
[
  {
    "critical": {
      "identity": {
        "docker-reference": "registry.suse.com/bci/bci-base"
      },
      "image": {
        "docker-manifest-digest": "sha256:52a828600279746ef669cf02a599660cd53faf4b2430a6b211d593c3add047f5"
      },
      "type": "cosign container image signature"
    },
    "optional": {
      "creator": "OBS"
    }
  }
]
```
{{</ tab >}}
{{< tab "using docker" >}}
```ShellSession
❯ docker run --rm -it -e COSIGN_EXPERIMENTAL=1 gcr.io/projectsigstore/cosign \
    verify --key https://ftp.suse.com/pub/projects/security/keys/container–key.pem \
    registry.suse.com/bci/bci-base:latest | tail -1 | jq
[
  {
    "critical": {
      "identity": {
        "docker-reference": "registry.suse.com/bci/bci-base"
      },
      "image": {
        "docker-manifest-digest": "sha256:52a828600279746ef669cf02a599660cd53faf4b2430a6b211d593c3add047f5"
      },
      "type": "cosign container image signature"
    },
    "optional": {
      "creator": "OBS"
    }
  }
]
```
{{< /tab >}}
{{< /tabs >}}


The more I dig into and support security the more I want my container images to
do the same. Happy to see these container images provide a great foundation and
security.

---
theme: default
title:  "code ready containers 102"
date:   2020-10-15 11:40:00
categories: containers openshift development local
excerpt_separator: <!--more-->
---

Once the CRC solution is [deployed properly](https://adrianriobo.github.io/containers/openshift/development/local/2020/10/14/code-ready-container-101.html), what is the internal architecture.

<!--more-->

## Internal architecture

As we saw the crc binary will interact with libvirt through the [crc-driver-libvirt](https://github.com/code-ready/machine-driver-libvirt).

**Not intended** Access the underlying VM for the SNC

```bash
ssh -i ~/.crc/machines/crc/id_rsa core@192.168.130.11
```

## Bibliography

[1. CRC Product Overview](https://developers.redhat.com/products/codeready-containers/overview)


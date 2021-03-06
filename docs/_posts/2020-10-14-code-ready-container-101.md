---
theme: default
title:  "code ready containers 101"
date:   2020-10-14 15:40:00
categories: containers openshift development local
excerpt_separator: <!--more-->
---

Initial steps with code ready containers solution. As an initial approach this tutorial will use nested virtualization to test the product (**DISCLAIMER: this is not the prefered way**).

Check vagrant [nested virtualization](https://adrianriobo.github.io/virtualization/testing/environment/2020/10/13/vagrant-nested-virtualization.html) to use vagrant as VMs manager.

![Stack](/assets/2020-10-14-code-ready-container-101_01.jpg)

<!--more-->

## Setup

Check virtualization extensions:  

```bash
cat /proc/cpuinfo | egrep "vmx|svm"
```

Fedora box is not prepared to use nested virtualization, so install required packages

```bash
sudo dnf -y install bridge-utils libvirt virt-install qemu-kvm
```

Check virtualization:

```bash
lsmod | grep kvm
```

Download [CRC + pull secret](https://cloud.redhat.com/openshift/install/crc/installer-provisioned)

Decompress crc:

```bash
tar -xf crc-linux-amd64.tar.xz
```

CRC setup:

```bash
crc setup
```

Configure pull secret file:

```bash
crc config set pull-secret-file ./pull-secret.txt
```

CRC start full information:

```bash
crc start
```

Configure oc to access cluster:

```bash
eval $(crc oc-env)
```

Access the cluster with:

```bash
oc login -u developer -p developer https://api.crc.testing:6443
oc login -u kubeadmin -p dpDFV-xamBW-kKAk3-Fi6Lg https://api.crc.testing:6443
```

## Issues

### Kernel / libvirt version (No issue anymore, but knowledge to handel old repositories on Fedora)

There is a problem with newer versions of kernels + nested virtualization + kvm_intel driver. Seems problem is related with libvirt versions so we need to downgrade to version 5.6.X:

Check fedora repositories mirrors, at mirrors [list](https://mirrors.fedoraproject.org/mirrorlist?repo=fedora-31&arch=x86_64)

Uninstall previous libvirt version:

```bash
sudo dnf remove libvirt
```

Add repository with previous libvirt version:

```bash
sudo dnf config-manager --add-repo https://mirror.infonline.de/fedora/linux/releases/31/Everything/x86_64/os/
```

Check GPG key for rpms on that repository and import the key, in other case an error will arise not allowing to install the rpms:

```bash
sudo find / -name "*GPG*-*31*"
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-31-x86_64
```

Check versions for libvirt:

```bash
dnf --showduplicates list libvirt
```

Install specific version:

```bash
sudo dnf install libvirt-5.6.0-4.fc31
```

## Bibliography

[1. CRC Product Overview](https://developers.redhat.com/products/codeready-containers/overview)

[2. KVM on Fedora](https://computingforgeeks.com/how-to-install-kvm-on-fedora/)

[3. Issue with nested virtualization kvm_intel](https://bugzilla.kernel.org/show_bug.cgi?id=203543)

[4. Fedora repositories management](https://docs.fedoraproject.org/en-US/quick-docs/adding-or-removing-software-repositories-in-fedora)

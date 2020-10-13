---
theme: default
title:  "code ready containers 101"
date:   2020-10-13 15:40:00
categories: containers openshift development local
---

## Overview

Initial steps with code ready containers solution. 

<!--more-->

## Setup

Download vagrant [from](https://www.vagrantup.com/downloads)  

Based on RHEL ensure KVM and libvird are properly configured:

```bash
sudo systemctl status libvirtd
```

KVM supports nested virtualization, in this case the provider used by vagrant to manage KVM is libvirt and it allows to configure nested virtualization with the config parameter nested as true:

```text
config.vm.provider :libvirt do |libvirt|
  # Enable KVM nested virtualization
  libvirt.nested = true
  libvirt.cpu_mode = "host-model"
end
```

### Issues

No usable default provider... libvirt requires plugin installation to manage the provider:

```bash
vagrant plugin install vagrant-libvirt
```

Due to some issue compiling the sources for the provider it is prefererd to install vagrant using dnf (this is due to the management for internal dependices, like embedded ruby)

```bash
sudo dnf install https://releases.hashicorp.com/vagrant/2.2.10/vagrant_2.2.10_x86_64.rpm

vagrant plugin install vagrant-libvirt
```

## Vagrantfile

```text
Vagrant.configure("2") do |config|
  config.vm.box = "fedora/32-cloud-base"
  config.vm.box_version = "32.20200422.0"
  
  config.vm.provider :libvirt do |libvirt|
    # Enable KVM nested virtualization
    libvirt.nested = true
    libvirt.cpu_mode = "host-model"
  end  
end
```

## Bibliography

[1. CRC Product Overview](https://developers.redhat.com/products/codeready-containers/overview)

---
theme: default
title:  "vagrant nested virtualization"
date:   2020-10-13 10:16:56
categories: virtualization testing environment
---

## Overview

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

## Bibliography

[1. RHEL virtualization](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/virtualization_deployment_and_administration_guide/index)

[2. Vagrant inside vagrant](https://nts.strzibny.name/inception-running-vagrant-inside-vagrant-with-kvm/)
---
theme: default
title:  "vagrant nested virtualization"
date:   2020-10-13 10:16:56
categories: virtualization testing environment
excerpt_separator: <!--more-->
---

As a testing environment we want to be able to provision local pre configured machines based on different OSs.  

As our target solution to be tested will use virtualization the virtual machines created should allow use virtualization (this is achieved through nested virtualization).  

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
sudo dnf install --disablerepo=* -y https://releases.hashicorp.com/vagrant/2.2.10/vagrant_2.2.10_x86_64.rpm

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

[1. RHEL virtualization](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/virtualization_deployment_and_administration_guide/index)

[2. Vagrant inside vagrant](https://nts.strzibny.name/inception-running-vagrant-inside-vagrant-with-kvm/)

[3. Vagrant libvirt plugin installation](https://github.com/vagrant-libvirt/vagrant-libvirt#installation)

[4. Vagrant issue installing libvirt plugin](https://github.com/vagrant-libvirt/vagrant-libvirt/issues/982#issuecomment-597470072)

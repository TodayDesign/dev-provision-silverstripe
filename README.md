dev-provision-silverstripe
=========

This is an ansible role for provisioning a Vagrant box for working on 
Silverstripe-based projects.

It's a fairly straightforward LEMP stack, configured to serve out of `/vagrant`
and includes node, yarn and gulp so that dev can happen from inside the VM.

This should only be used on a virtual machine for local development. It's not
suitable at all for any kind of production or staging server.

Role Variables
--------------

MySQL will be set up with `mysql_username` and `mysql_password` both set to "vagrant".

Dependencies
------------

This role depends on nodesource.node, geerlingguy.git and geerlingguy.composer.

Example Playbook
----------------

A sample Vagrantfile with a corresponding provision.yml file is included here.

### Vagrantfile:

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "debian/jessie64"
  config.vm.hostname = "merrihealth"

  config.vm.synced_folder ".", "/vagrant", type: "virtualbox"

  config.vm.network "forwarded_port", guest: 80, host: 8111

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = 'provision.yml'
  end
end
```

### provision.yml:

```
---
- hosts: all
  become: yes
  become_method: sudo
  roles:
    - StudioThick.dev-provision-silverstripe
```

License
-------

BSD

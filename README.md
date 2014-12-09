orchestration-middleware-swift
==============================

Provision swift middleware for authentication.

## Prereqs

Install the following applications on your local machine first:

 * [Vagrant](http://vagrantup.com)
 * [Vagrant openstack plugin] (https://github.com/ggiamarchi/vagrant-openstack-provider)
 * [Ansible](http://ansibleworks.com)
 * [Puppet] (http://puppetlabs.com)
 
## Quick Start

After installing the applications above, clone this repository and then make:

```
$ cd orchestration-middleware-swift
$ git clone https://github.com/f2brossi/puppet-postfix-gmail.git
$ git clone https://github.com/f2brossi/jboss7-standalone-vagrant.git
$ git clone https://github.com/f2brossi/ansible-apache-swift.git
$ git clone https://github.com/f2brossi/vagrant-devstack-swift.git
...
```

Then update the Vagrantfile given, filling in your information where necessary by replacing each
ENV['OS_XXXX'] according to your openstack provider account and credentials.

And finally:

```
$ vagrant up --provider=openstack
...
```

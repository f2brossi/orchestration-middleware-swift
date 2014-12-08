orchestration-middleware-swift
==============================

Middleware provisioning for swift devstack storage

## Prereqs

Install the following applications on your local machine first:

 * [Vagrant](http://vagrantup.com)
 * [Vagrant openstack plugin] (https://github.com/ggiamarchi/vagrant-openstack-provider)
 * [Ansible](http://ansibleworks.com)
 * [Puppet]
 
## Quick Start

After installing the applications above, the you neeed to clone the fowwolling repositories:

git clone https://github.com/f2brossi/puppet-postfix-gmail.git
git clone https://github.com/f2brossi/jboss7-standalone-vagrant.git
git clone https://github.com/f2brossi/ansible-apache-swift.git
git clone https://github.com/f2brossi/vagrant-devstack-swift.git


And tehn update the Vagrantfile given, filling in your information where necessary by replacing each
ENV['OS_XXXX'] according to your openstack provider account and credentials.



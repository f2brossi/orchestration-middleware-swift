require 'vagrant-openstack-provider'

Vagrant.configure("2") do |config|

  config.vm.box = "openstack"
  config.vm.box_url = "https://github.com/ggiamarchi/vagrant-openstack/raw/master/source/dummy.box"

  config.ssh.private_key_path = ENV['OS_KEYPAIR_PRIVATE_KEY']
  config.ssh.shell = "bash"

  config.vm.provider :openstack do |os|
    os.username = ENV['OS_USERNAME']
    os.password = ENV['OS_PASSWORD']
    os.openstack_auth_url = ENV['OS_AUTH_URL']
    os.openstack_compute_url = ENV['OS_COMPUTE_URL']
    os.openstack_network_url = ENV['OS_NETWORK_URL']
    os.tenant_name = ENV['OS_TENANT_NAME']
    os.keypair_name = ENV['OS_KEYPAIR_NAME']
  end

  config.vm.define 'test' do |test|
    test.vm.provider :openstack do |os|
      os.server_name = "test-middleware"
      os.floating_ip = "ENV['OS_FLOATING_IP']
      os.flavor = '2_vCPU_RAM_4G_HD_10G'
      os.image = 'ubuntu-12.04_x86-64_3.11-DEPRECATED'
      os.ssh_username = "stack"
      os.networks = ['net']
    end
  end

  # Install puppet  
  config.vm.provision "shell", path: "puppet-postfix-gmail/installPuppetOnUbuntu.sh", privileged: "true"

  # set my credentials
  Vagrant.configure("2") do |config|
   config.vm.provision "shell" do |s|
     s.inline = "sed 's/yourgmailaccount/mygmailaccount/' <puppet/init-postfix.pp >puppet/myinit-postfix.pp"
   end
  end

  Vagrant.configure("2") do |config|
   config.vm.provision "shell" do |s|
     s.inline = "sed 's/yourpassword/mypassword/' <puppet/init-postfix.pp >puppet/myinit-postfix.pp"
   end
  end

  # Update /etc/hosts with hostname @ip
  config.vm.provision "shell" do |s|
    s.inline = "echo $1 $2>> /etc/hosts"
    s.args = "'127.0.0.1' 'test-middleware'"
    s.privileged = "true"   
  end

  config.vm.provision "puppet" do |puppet|
      puppet.module_path = "puppet-postfix-gmail/puppet/modules"
      puppet.manifests_path = "."
      puppet.manifest_file = "puppet-postfix-gmail/puppet/init-postfix.pp"
      puppet.options = "--verbose --debug"
  end

 # update hosts with the floating IP
 Vagrant.configure("2") do |config|
   config.vm.provision "shell" do |s|
     s.inline =	"echo $OS_FLOATING_IP >> jboss7-ansible-jboss/hosts"
   end
  end


 # Install ansible
 config.vm.provision "shell", path: "jboss7-standalone-vagrant/installAnsibleOnUbuntu.sh", privileged: "true"

 config.vm.provision :ansible do |ansible|
  ansible.playbook = "jboss7-standalone-vagrant/ansible-jboss/jboss.yml"
  ansible.verbose = "vv"
  ansible.limit = 'all'
  ansible.sudo = true
 end

  # update /etc/hosts with devstack @ip
  config.vm.provision "shell" do |s|
        s.inline = "echo $1 $2>> /etc/hosts"
        s.args = "ENV['OS_DEVSTACK_URL'] 'openstack.ne.local'"
	s.privileged = "true"                 
  end

  # provision and deployment with ansible
  config.vm.provision :ansible do |ansible|
      ansible.playbook = "ansible-apache-swift/ansible-apacheproxy/apache.yml"
      ansible.verbose = "vv"
      ansible.limit = 'all'
      ansible.sudo = true 
  end

# start swift devstack
config.vm.provider :openstack do |os|
    os.username = ENV['OS_USERNAME']
    os.password = ENV['OS_PASSWORD']
    os.openstack_auth_url = ENV['OS_AUTH_URL']
    os.openstack_compute_url = ENV['OS_COMPUTE_URL']
    os.openstack_network_url = ENV['OS_NETWORK_URL']
    os.tenant_name = ENV['OS_TENANT_NAME']
    os.keypair_name = ENV['OS_KEYPAIR_NAME']
  end

  config.vm.define 'test' do |test|
    test.vm.provider :openstack do |os|
      os.server_name = "test-devstack"
      os.floating_ip = ENV['OS_DEVSTACK_URL']
      os.flavor = '4_vCPU_RAM_8G_HD_10G'
      os.image = 'ubuntu-12.04_x86-64_3.11-DEPRECATED'
      os.ssh_username = "stack"
      os.networks = ['net']
    end
  end

   # Install puppet  
  config.vm.provision "shell", path: "vagrant-devstack-swift/installPuppetOnUbuntu.sh", privileged: "true"

  # update /etc/hosts  
  config.vm.provision "shell", path: "vagrant-devstack-swift/hosts.sh", privileged: "true"

  config.vm.provision "puppet" do |puppet|
      puppet.module_path = "vagrant-devstack-swift/puppet/modules"
      puppet.manifests_path = "."
      puppet.manifest_file = "vagrant-devstack-swift/puppet/init.pp"
      puppet.options = "--verbose --debug"
  end

end

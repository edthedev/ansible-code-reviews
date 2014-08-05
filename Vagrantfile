# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "chef/centos-6.5"

# Gerrit web interface.
  config.vm.network :forwarded_port, guest: 8080, host: 8080
# Gerrit SSH access.
  config.vm.network :forwarded_port, guest: 2221, host: 2221
  config.vm.network :forwarded_port, guest: 29418, host: 29418

  config.vm.provider "virtualbox" do |v|
    # v.memory = 4096
    v.cpus = 2
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
	ansible.verbose = 'vvv'
# For debugging
#     ansible.start_at_task = 'python stuff'
#    ansible.start_at_task = 'gerrit db user'
  end
end

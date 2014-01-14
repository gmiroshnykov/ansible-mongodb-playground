# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  config.vm.define "master", :primary => true do |master|
    master.vm.hostname = "master.mongodb.dev"
    master.vm.network :private_network, ip: "10.0.40.2"
  end

  config.vm.define "slave" do |slave|
    slave.vm.hostname = "slave.mongodb.dev"
    slave.vm.network :private_network, ip: "10.0.40.3"

    # FIXME: private network needs some time to get up
    config.vm.provision :shell, inline: "/bin/sleep 10"

    # we only add Ansible provisioning to the last VM,
    # see: https://github.com/mitchellh/vagrant/issues/1784#issuecomment-21041577
    slave.vm.provision :ansible do |ansible|
      ansible.playbook = "ansible/all.yml"
      ansible.inventory_path = "ansible/vagrant"
      ansible.verbose = true
    end
  end

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "512"]
  end
end

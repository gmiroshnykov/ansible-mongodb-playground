# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"
  config.cache.scope = :machine

  config.ssh.forward_agent = true

  config.vm.define "master", :primary => true do |master|
    master.vm.hostname = "master.mongodb.dev"
    master.vm.network :private_network, ip: "10.0.40.2"
    master.vm.synced_folder ".", "/opt/rebbix-analytics", :nfs => true
  end

  config.vm.define "slave" do |slave|
    slave.vm.hostname = "slave.mongodb.dev"
    slave.vm.network :private_network, ip: "10.0.40.3"

    # we only add Ansible provisioning to the last VM,
    # see: https://github.com/mitchellh/vagrant/issues/1784#issuecomment-21041577
    slave.vm.provision :ansible do |ansible|
      ansible.playbook = "ansible/all.yml"
      ansible.inventory_path = "ansible/vagrant"
      ansible.verbose = true
    end
  end

  cpus = 1
  memory = ENV["MEMORY"] ? ENV["MEMORY"] : "512"

  config.vm.provider :virtualbox do |vb|
    vb.gui = true if ENV["DEBUG"]
    vb.customize ["modifyvm", :id, "--memory", memory]
    vb.customize ["modifyvm", :id, "--cpus", cpus]

    # seriously, there are no typos on the next line!
    vb.customize ["guestproperty", "set", :id, "/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold", "1000"]
  end

  config.vm.provider :vmware_fusion do |v|
    v.gui = true if ENV["DEBUG"]
    v.vmx["memsize"] = memory
    v.vmx["numvcpus"] = cpus
  end
end

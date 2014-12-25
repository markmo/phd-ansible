# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "chef/centos-6.5"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
  # config.ssh.forward_agent = true

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true

  config.vm.provider "virtualbox" do |v|
    v.customize ["modifyvm", :id, "--cpus", "2", "--memory", "2048"]
  end

  config.vm.define "admin" do |admin|
    admin.vm.network "private_network", ip: "192.168.50.4"
    admin.vm.hostname = "admin.snbc.io"
  end

  config.vm.define "master1" do |master1|
    master1.vm.network "private_network", ip: "192.168.50.5"
    master1.vm.hostname = "master1.snbc.io"
  end

  config.vm.define "slave1" do |slave1|
    slave1.vm.network "private_network", ip: "192.168.50.6"
    slave1.vm.hostname = "slave1.snbc.io"
  end

  config.vm.define "slave2" do |slave2|
    slave2.vm.network "private_network", ip: "192.168.50.7"
    slave2.vm.hostname = "slave2.snbc.io"

    # An Ansible hack (https://github.com/mitchellh/vagrant/issues/1784).
    # In a multi-vm configuration, VM machines will be started according to
    # their declaration order. Inserting the Ansible declaration here, makes
    # sure that the provisioning process will be executed only after all the
    # VMs are up and running.
    slave2.vm.provision "ansible" do |ansible|
      ansible.inventory_path = "inventory"
      ansible.verbose = "vvvv"
      ansible.sudo = true
      ansible.playbook = "playbook.yml"
      ansible.extra_vars = { ansible_ssh_user: 'vagrant' }

      # Disable default limit (required with Vagrant 1.5+)
      ansible.limit = 'all'
    end

  end

end
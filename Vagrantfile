# coding: utf-8
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  if Vagrant.has_plugin?("vagrant-cachier")
    # Configure cached packages to be shared between instances of the same base box.
    # More info on http://fgrehm.viewdocs.io/vagrant-cachier/usage
    config.cache.scope = :box
  end

  config.ssh.insert_key = false

  cluster = {
    "master" =>  { :ip => "192.168.33.11", :mem => 1024,  :cpu => 1 }
  }

  cluster.each_with_index do | (hostname, info), index |
    config.vm.synced_folder ".", "/vagrant", disabled: false
    config.vm.define hostname do | host |
      host.vm.box = "bento/ubuntu-16.04"
      host.vm.provider "virtualbox" do |v|
        v.memory = info[:mem]
        v.cpus   = info[:cpu]
      end
      host.vm.hostname = hostname
      host.vm.network "private_network", ip: info[:ip]

      config.vm.provision "shell", inline: <<-SHELL
        apt-get update -yyd
        apt-get install python -y
      SHELL
        # config.vm.provision :ansible do |ansible|
        #   # Disable default limit to connect to all the machines
        #   ansible.limit = 'all'
        #   ansible.playbook = "provision/init_dev.yml"
        #   ansible.inventory_path = "provision/inventory"
        # end
      # end
    end
  end

  config.vm.box_check_update = false
end

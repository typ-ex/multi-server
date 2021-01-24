# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Base VM OS configuration
  config.vm.box = "geerlingguy/ubuntu2004"
  config.vm.synced_folder '.', '/vagrant', disabled: true
  config.ssh.insert_key = false

  config.vm.provider :virtualbox do |v|
    v.memory = 512
    v.cpus = 1
    v.linked_clone = true
  end

  # Define 4 VM's with static private IPs
  boxes = [
    { :name => "nodejs1", :ip => "10.19.0.92" },
    { :name => "nodejs2", :ip => "10.19.0.93" },
    { :name => "nodejs3", :ip => "10.19.0.94" },
    { :name => "nodejs4", :ip => "10.19.0.95" }
]

  # Provision each VMs
  boxes.each do |opts|
    config.vm.define opts[:name] do |config|
      config.vm.hostname = opts[:name]
      config.vm.network :private_network, ip: opts[:ip]

      # Provision all of the VMs using ansible after the last VM is up
      if opts[:name] == "nodejs4"
        config.vm.provision "ansible" do |ansible|
          ansible.playbook = "playbooks/main.yml"
          ansible.inventory_path = "inventory"
          ansible.limit = "all"
        end
      end
    end
  end

end

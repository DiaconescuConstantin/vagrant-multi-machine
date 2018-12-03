# -*- mode: ruby -*-
# vi: set ft=ruby :

# check for vagrant-docker-compose plugin dependency
unless Vagrant.has_plugin?("vagrant-docker-compose")
    system("vagrant plugin install vagrant-docker-compose")
    puts "Dependencies installed, please try the command again."
    exit
end

Vagrant.configure("2") do |config|

    # install docker-compose into the VM and run the docker-compose.yml file - if it exists - whenever the VM starts
    # config.vm.provision :docker_compose, yml: "/vagrant/docker-compose.yml", run:"always"
    config.vm.define "leader", primary: true do |leader|
        leader.vm.box = "ubuntu/trusty64"
        leader.vm.network "private_network", ip: "192.168.50.2"

        leader.vm.provider "virtualbox" do |vb|
            # Display the VirtualBox GUI when booting the machine
            # vb.gui = true
            vb.name = 'docker-swarm-leader'
    
            # Customize the amount of memory on the VM:
            vb.memory = "2048"
            vb.cpus = 2
    
            # This setting makes the NAT engine use the host's resolver mechanisms to handle DNS requests. See <https://www.virtualbox.org/manual/ch09.html#nat-adv-dns>
            vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
            # This setting makes the NAT engine proxy all guest DNS requests to the host's DNS servers. See <https://www.virtualbox.org/manual/ch09.html#nat-adv-dns>
            vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
        end

        leader.vm.provision "shell", inline: <<-SHELL
          apt-get update
        SHELL

        # set up Docker in the new VM:
        leader.vm.provision :docker

        # install docker-compose into the VM
        leader.vm.provision :docker_compose

    end

    config.vm.define "follower" do |follower|
        follower.vm.box = "ubuntu/trusty64"
        follower.vm.network "private_network", ip: "192.168.50.3"

        follower.vm.provider "virtualbox" do |vb|
            # Display the VirtualBox GUI when booting the machine
            # vb.gui = true
            vb.name = 'docker-swarm-follower'
    
            # Customize the amount of memory on the VM:
            vb.memory = "2048"
            vb.cpus = 2
    
            # This setting makes the NAT engine use the host's resolver mechanisms to handle DNS requests. See <https://www.virtualbox.org/manual/ch09.html#nat-adv-dns>
            vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
            # This setting makes the NAT engine proxy all guest DNS requests to the host's DNS servers. See <https://www.virtualbox.org/manual/ch09.html#nat-adv-dns>
            vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
        end

        follower.vm.provision "shell", inline: <<-SHELL
          apt-get update
        SHELL
      
        # set up Docker in the new VM:
        follower.vm.provision :docker

        # install docker-compose into the VM
        follower.vm.provision :docker_compose
    end
end
  
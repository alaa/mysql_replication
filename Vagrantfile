# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box_check_update = false
  #config.ssh.private_key_path  = "~/.ssh/id_rsa"

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration
  config.vm.provider "virtualbox" do |vm|
    vm.memory = "1024"
  end

  def shell
    <<-SHELL
    sudo apt-get update
    sudo apt-get build-dep -y mysql-server
    wget http://dev.mysql.com/get/mysql-apt-config_0.3.2-1ubuntu14.10_all.deb /tmp
    sudo dpkg -iy /tmp/mysql-apt-config_0.3.2-1ubuntu14.10_all.deb
    sudo apt-get update

    debconf-set-selections <<< "mysql-server mysql-server/root_password password root"
    debconf-set-selections <<< "mysql-server mysql-server/root_password_again password root"
    sudo apt-get install -y mysql-server
    sudo service mysql restart
    SHELL
  end


  config.vm.define "master" do |master|
    master.vm.box = "master"
    master.vm.box = "ubuntu-14.10-64"
    master.vm.network "private_network", ip: "10.10.10.10"
    master.vm.provision "shell", inline: shell
  end

  config.vm.define "slave" do |slave|
    slave.vm.box = "slave"
    slave.vm.box = "ubuntu-14.10-64"
    slave.vm.network "private_network", ip: "10.10.10.11"
    slave.vm.provision "shell", inline: shell
  end

end

# -*- mode: ruby -*-
# vi: set ft=ruby :

# Multi instance
VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config| 
  config.vm.define "backend" do |backend|
    backend.vm.box = "ubuntu-12.04-mysql" 
    config.vm.box_url = "../../../DevOps_webacademy/week1/packer-templates-develop/ubuntu-12.04/ubuntu-12-04-x64-virtualbox.box"
    backend.vm.host_name = 'backend.test.com' 
    backend.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1024"] 
      vb.customize ["modifyvm", :id, "--cpus", "1"]
    end
    backend.vm.network "private_network", ip: "172.16.0.11" 
    backend.vm.provision "shell", inline: <<-SHELL
      echo "CREATE DATABASE IF NOT EXISTS wordpress DEFAULT CHARACTER SET \
      'utf8' COLLATE 'utf8_unicode_ci';" | mysql --user=root --password=vagrant
      echo "GRANT ALL ON wordpress.* TO 'wordpress_user'@'172.16.0.21' IDENTIFIED BY 'XiBMFjQgqO66ACI';" | mysql \
      --user=root --password=vagrant
    SHELL
  end
  
  config.vm.define "web" do |web|
    web.vm.box = "centos-6-4-x64-httpd"
    web.vm.box_url = "../../../DevOps_webacademy/week1/packer-templates-develop/centos-6.4/centos-6-4-x64-virtualbox.box"
    web.vm.host_name = 'web.test.com' 
    web.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--memory", "512"]
      vb.customize ["modifyvm", :id, "--cpus", "1"]
    end
    web.vm.network "private_network", ip: "172.16.0.21" 
    web.vm.network "public_network", bridge: 'en0: Wi-Fi'
    web.vm.synced_folder "/Users/gipson/Documents/Study/DevOps_webacademy/vagrant/wp_vagrant/wordpress-sources/", "/var/www/wordpress"
    #web.vm.network "forwarded_port", guest: 80, host: 8080
  end
  
end
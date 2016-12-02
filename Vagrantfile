# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

  config.vm.box = "bento/ubuntu-16.04"

  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = "512"
    vb.cpus = "1"
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y sqlite3 ruby-sequel ruby-chronic ruby-rspec
    gem install timetrap
  SHELL

end

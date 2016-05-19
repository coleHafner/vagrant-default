# -*- mode: ruby -*-
# vi: set ft=ruby :

#INSTALL PLUGINS
required_plugins = %w( vagrant-hostmanager )
required_plugins.each do |plugin|
    exec "vagrant plugin install #{plugin};vagrant #{ARGV.join(" ")}" unless Vagrant.has_plugin? plugin || ARGV[0] == 'plugin'
end

Vagrant.configure(2) do |config|

  #BOX
  config.vm.box = "ubuntu/trusty64"
  config.vm.box_check_update = true

  #NETWORKING
  config.vm.network "private_network", ip: LOCAL_ARGS["guest_ip"]
  config.ssh.forward_agent = true

  # Forward any ports necessary.
  config.vm.network "forwarded_port", guest: 9001, host: 9001
  config.vm.network "forwarded_port", guest: 35729, host: 35729

  #SYNC
  config.vm.synced_folder "../", "/var/www/" + LOCAL_ARGS["guest_hostname"], type: "nfs", mount_options: ['actimeo=1']

  #HOSTNAME CONFIG(requires vagrant-hostmanager)
  config.vm.hostname = LOCAL_ARGS["guest_hostname"]
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true

  #PROVIDER (AKA VM)
  config.ssh.forward_agent = true
  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
    v.cpus = 2
  end

  #BOOTSTRAP
  config.vm.provision :shell, path: "provisions/main.sh", :args => [LOCAL_ARGS["guest_ip"], LOCAL_ARGS["guest_hostname"], LOCAL_ARGS["app_root"], LOCAL_ARGS["client_root"]]
end

#LOCAL VAGRANT CONFIG
if File.exists?('config.rb') then
	localconfig = File.read 'config.rb'
	eval localconfig
end
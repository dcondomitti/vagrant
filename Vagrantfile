# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'yaml'
require 'socket'

# Opscode platform requires unique hostnames for each node registered so
# the Vagrantfile needs to include a user's name in the node's name. We
# can use someone's github username as long as they have devtools installed
# properly. 
# 
# This assumes people only use one machine, if you use more than one you'll
# need to copy /etc/chef/client.pem to your other VMs or manually override
# the node name.
begin
  pp_config = YAML.load_file(File.expand_path(File.join('~', '.pp', 'config.yml')))
  username = pp_config['username']
  pushparty_token = pp_config['token']
  raise "Missing username and/or pushparty token" unless username && pushparty_token
rescue => e
  puts "Tried to get your devtools config but could not read the file or settings were missing."
  puts "Please go to http://pushparty.paperlesspost.com/users/settings for more info."
  puts "Original error: #{e}"
end

shared_path = ENV['PAPERLESS_MOUNT'] || (File.expand_path('../'))
Dir.mkdir shared_path unless Dir.exists? shared_path

hostname = Socket.gethostname.split('.')[0] rescue nil
vm_host_name = ENV['PAPERLESS_VAGRANTHOST'] || [hostname, username, "vagrant.paperlesspost.com"].compact.join('.')

uid = %x[id -u].chomp

default_attributes = {
  :devtools_config => pp_config
}

default_attributes[:vagrant_db_provision] = true if ENV['DBPROVISION']
default_attributes[:vagrant_update_repos] = true if ENV['UPDATEREPOS']
default_attributes[:vagrant_setup_devtools] = true if ENV['SETUPDEVTOOLS']

Vagrant::Config.run do |config|
  config.vm.box = 'paperless-4.1.16'
  config.vm.box_url = 'http://nybuntu.paperlesspost.com/paperless-4.1.16.box'

  config.ssh.username = "paperless"
  config.vm.host_name = vm_host_name
  config.nfs.map_uid = uid 
  config.vm.share_folder "paperlesspost", "/opt/src/paperlesspost", shared_path, :nfs => true

  config.vm.network :hostonly, "1.0.0.254"

  config.vm.provision :chef_client do |chef|
    chef.chef_server_url = "https://api.opscode.com/organizations/paperlesspost"
    chef.validation_client_name = "paperlesspost-validator"
    chef.validation_key_path = ".chef/paperlesspost-validator.pem"
    chef.add_role("flat")
    chef.environment = "development"
    chef.json = default_attributes
  end
end

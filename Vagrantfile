# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'yaml'
require 'socket'
require 'net/http'

box_name    = 'paperless-4.1.16'
box_url     = 'https://paperless.interval.io.s3.amazonaws.com/paperless-4.1.16.box?AWSAccessKeyId=AKIAJX2DBMWSKWJN2JXA&Expires=1340159480&Signature=Sc8P98PjV0%2BR99xHvEwwVCAuJg8%3D'

# This is only accessible from within the office; the VPN doesn't have the right
# routes to make it accessible from outside the office so we'll fall back to S3
# especially since that's probably faster anyway.
box_office_url = 'http://nybuntu.paperlesspost.com/paperless-4.1.16.box'

# Opscode platform requires unique hostnames for each node registered so
# the Vagrantfile needs to include a user's name in the node's name. We
# can use someone's github username as long as they have devtools installed
# properly.
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

# We allow the shared mount to be overridden (if you don't want to smash your 
# existing install) and create it if it doesn't exist.
shared_path = ENV['PAPERLESS_MOUNT'] || (File.expand_path('../'))
Dir.mkdir shared_path unless Dir.exists? shared_path

# The VM hostname now includes the real OS X hostname along with the user's
# github username in the format of #{osx_hostname}.#{github_username}.vagrant.paperlesspost.com
hostname = Socket.gethostname.split('.')[0] rescue nil
vm_host_name = ENV['PAPERLESS_VAGRANTHOST'] || [hostname, username, "vagrant.paperlesspost.com"].compact.join('.')

# This is the initial JSON passed into chef-client as the node attributes
# and whenever you run vagrant provision
default_attributes = {
  :devtools_config => pp_config
}

# Forces a db migration, production slice import and thinking sphinx
# reindex
default_attributes[:vagrant_db_provision] = true if ENV['DBPROVISION']

# Forces all repositories to be updated through PP (includes code sync
# bundle, SWF compilation, migrations, etc)
default_attributes[:vagrant_update_repos] = true if ENV['UPDATEREPOS']

# Forces devtools to be set up again.
default_attributes[:vagrant_setup_devtools] = true if ENV['SETUPDEVTOOLS']

# When you're in the office we don't want to hit S3 for a file that we can stream
# at like 8MB/s. Test to see if nybuntu.paperlesspost.com is reachable and fall-
# back to S3 if necessary. The period on the end is necessary because otherwise
# the dns suffixes provided by vagrant (vagrantup.com) will be searched and it's
# wildcarded to Github Pages.
http = Net::HTTP.new('nybuntu.paperlesspost.com.', 80)
http.open_timeout = 1
http.read_timeout = 1

begin
  box_url = box_office_url if http.get('/').code == '200'
rescue Exception => e
  # Don't care why it failed, just that we need to stay with the S3 url
end

Vagrant::Config.run do |config|
  config.vm.box = box_name
  config.ssh.username = 'paperless'
  config.ssh.forward_agent = true
  config.vm.box_url = box_url
  config.vm.host_name = vm_host_name

  # This private network interface (1.0.0.1 in the VM) is for sharing NFS
  config.vm.network :hostonly, "1.0.0.254"
  config.vm.share_folder "paperlesspost", "/opt/src/paperlesspost", shared_path, :nfs => true
  config.nfs.map_uid = uid = %x[id -u].chomp

  # This pair of ports is forwarded to the NAT'ed interface within the VM so that
  # you can access the mobile web interface by the IP of your host without
  # needing to add it as m.local.paperlesspost.com on your other device.
  config.vm.forward_port 9080, 9080
  config.vm.forward_port 9443, 9443

  config.vm.provision :chef_client do |chef|
    chef.chef_server_url = "https://api.opscode.com/organizations/paperlesspost"
    chef.validation_client_name = "paperlesspost-validator"
    chef.validation_key_path = ".chef/paperlesspost-validator.pem"
    chef.add_role("flat")
    chef.environment = "development"
    chef.json = default_attributes
  end
end

# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'yaml'
require 'httparty'

class DevtoolsNotConfiguredException < Exception; end

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
  username = pp_config['github_username']
  raise DevtoolsNotConfiguredException unless username
rescue Errno::ENOENT, DevtoolsNotConfiguredException => e 
  username = %x[whoami].chomp
  puts "Falling back to machine username of #{username}"
end

shared_path = ENV['PAPERLESS_MOUNT'] || (File.expand_path('../' + 'vagrant-shared'))
Dir.mkdir shared_path unless Dir.exists? shared_path

# Download the validator if it's not already present


Vagrant::Config.run do |config|
  config.vm.box = 'paperless-final'
  config.vm.box_url = 'https://paperless.interval.io.s3.amazonaws.com/paperless-final.box?AWSAccessKeyId=AKIAJX2DBMWSKWJN2JXA&Expires=1336408598&Signature=G2j6tgiyA66xdSUmSs9ies0Hgc8%3D'

  config.ssh.username = "paperless"
  config.vm.host_name = "#{username}-newtest.vagrant"
  config.vm.share_folder "paperlesspost", "/opt/src/paperlesspost", shared_path, :nfs => true

  config.vm.network :hostonly, "1.0.0.254"

  config.vm.provision :chef_client do |chef|
    chef.chef_server_url = "https://api.opscode.com/organizations/paperlesspost"
    chef.validation_client_name = "paperlesspost-validator"
    chef.validation_key_path = ".chef/paperlesspost-validator.pem"
    chef.add_role("flat")
    chef.environment = "development"
  end

end

# Let's duckpunch the provisioner to pull the validator PEM down from S3 before
# attempting to provision the VM.
module VagrantPlugins
  module Chef
    module Provisioner
      class ChefClient
        class Config
          attr_accessor :aws_s3_bucket_name, :aws_access_key, :aws_secret_access_key, :validator_name, :box_name

          def prepare
            # Check for IAM S3 Keys

            # Sign URL

            # Download validator PEM

            # Save to disk
          end

          def cleanup
            # Remove validator from disk
          end

          def sign_validator_url
            s3.directories.new(:key => BUCKET).files.new(:key => object).url(expiry)
          end

          def download_validator
            validator_path = ENV['PAPERLESS_VALIDATOR'] || File.expand_path('.chef/' + 'paperless-validator.pem')
            unless validator_exists?
              response = HTTParty.get('https://whatevers3bucket.s3.amazonaws.com/paperless-validator.pem')
              open(validator_path, 'wb') { |file| file.write response.body }
            end            
          end

          def validator_exists?
            File.exists? validator_path
          end
        end
      end
    end
  end
end

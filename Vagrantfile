# -*- mode: ruby -*-
# vi: set ft=ruby :

# see: https://github.com/opscode/bento
# and: https://github.com/schisamo/vagrant-omnibus

system('chefvm use agilewebops')

def define_node(config, node_name, ip_address=nil, role=nil)
  config.vm.define node_name do |node|
    node.vm.hostname = node_name.to_s
    node.vm.network :private_network, ip: ip_address if ip_address     
    node.vm.provision :chef_client do |chef|
      chef.node_name = node_name.to_s
      chef.chef_server_url = "https://api.opscode.com/organizations/ejo"
      chef.validation_key_path = "/home/juetten/repositories/chef-repo/.chef/ejo-validator.pem"
      chef.validation_client_name = "ejo-validator"
      chef.provisioning_path = "/etc/chef"
      chef.add_role role if role
    end
  end
end

Vagrant.configure("2") do |config|

  config.berkshelf.enabled = true
  
  config.omnibus.chef_version = :latest

  config.vm.box = "opscode-centos-7.0"
  config.vm.box_url = "http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_centos-7.0_chef-provisionerless.box"

  define_node(config, :server)
end

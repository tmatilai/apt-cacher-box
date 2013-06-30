Vagrant.require_plugin 'vagrant-berkshelf'
Vagrant.require_plugin 'vagrant-omnibus'

IP  = ENV.fetch('APT_CACHER_IP', '192.168.33.200')
BOX = ENV.fetch('BOX', 'debian-7')

Vagrant.configure("2") do |config|
  config.vm.box      = BOX
  config.vm.hostname = 'apt-cacher'

  config.vm.network :private_network, ip: IP

  # Disable default share
  config.vm.synced_folder '.', '/vagrant', id: 'vagrant-root', disabled: true

  # Configure plugins
  config.berkshelf.enabled = true
  config.omnibus.chef_version = :latest

  # Override global vagrant-proxyconf settings
  if defined? VagrantPlugins::ProxyConf
    config.apt_proxy.http  = false
    config.apt_proxy.https = false
  end

  # Install apt-cacher-ng
  # Do not configure local system to use the proxy as the cacher-client makes
  # Apt to use the not-yet-installed proxy
  config.vm.provision :chef_solo do |chef|
    chef.add_recipe 'apt::default'
    chef.add_recipe 'apt::cacher-ng'
  end
end

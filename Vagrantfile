Vagrant.configure("2") do |config|
  config.vm.box = "raring"
  #config.vm.box_url = "http://bit.ly/vagrant-lxc-quantal64-2013-07-12"
  config.vm.box_url = "http://dl.dropbox.com/u/13510779/lxc-raring-amd64-2013-05-08.box"
  
  #config.vm.define :drupal do |drupal_config|
    #drupal_config.vm.forward_port 80, 8080 # lxc-drupal has no support yet
  #end

  config.vm.provider :lxc do |lxc|
  # Same effect as as 'customize ["modifyvm", :id, "--memory", "1024"]' for VirtualBox
    lxc.customize 'cgroup.memory.limit_in_bytes', '1024M'
  end

  config.ssh.forward_agent = true
  
  config.vm.synced_folder "./", "/vagrant", id: "vagrant-root"

  config.vm.provision :shell, :inline => "sudo apt-get update; touch /etc/puppet/hiera.yaml"
  config.vm.provision :shell, :inline => 'echo -e "mysql_root_password=puppetdrupal
controluser_password=puppetdrupal" > /etc/phpmyadmin.facts;'
  config.vm.provision :shell, :inline => "chmod o+w /vagrant/drupal/sites/default/settings.php /vagrant/drupal/sites/default/files"
  config.vm.provision :shell, :inline => "mkdir -p /var/www ; cp -anr /vagrant/drupal /var/www/drupal"

  config.vm.provision :puppet do |puppet|
    puppet.manifests_path = "manifests"
    puppet.module_path = "modules"
    puppet.options = ['--verbose']
  end
end

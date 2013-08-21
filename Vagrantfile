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

  config.vm.provision :shell, :inline => "apt-get update; touch /etc/puppet/hiera.yaml"
  config.vm.provision :shell, :inline => 'echo -e "mysql_root_password=puppetdrupal
controluser_password=puppetdrupal" > /etc/phpmyadmin.facts;'


  config.vm.provision :puppet do |puppet|
    puppet.manifests_path = "manifests"
    puppet.module_path = "modules"
    puppet.options = ['--verbose']
  end
  config.vm.provision :shell, :inline => "[[ ! -f /var/www/sites/default/settings.php ]] && (rm -rf /var/www/ ; cd /var ; drush dl drupal 2>&1 >/dev/null ; mv /var/drupal*/ /var/www/)"
  config.vm.provision :shell, :inline => "cp -anr /var/www/sites/default/default.settings.php /var/www/sites/default/settings.php;"
  config.vm.provision :shell, :inline => "chmod a+w /var/www/sites/default ; mkdir /var/www/sites/default/files ; chown -R www-data:www-data /var/www/"
  config.vm.provision :shell, :inline => "apt-get clean; rm -rf /var/www/drupal/.git"
end

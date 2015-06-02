# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure("2") do |config|
  config.vm.box = "fgrehm-VAGRANTSLASH-trusty64-lxc"
  config.vm.box_url = "https://atlas.hashicorp.com/fgrehm/boxes/trusty64-lxc/versions/1.2.0/providers/lxc.box"
  
  #config.vm.define :drupal do |drupal_config|
    #drupal_config.vm.forward_port 80, 8080 # lxc-drupal has no support yet
  #end

  config.vm.provider :lxc do |lxc|
  # Same effect as as 'customize ["modifyvm", :id, "--memory", "1024"]' for VirtualBox
    lxc.customize 'cgroup.memory.limit_in_bytes', '1024M'
  end

  config.ssh.forward_agent = true
  
  config.vm.synced_folder "./", "/vagrant", id: "vagrant-root"

  config.vm.provision :shell, :inline => "apt-get update"
  config.vm.provision :shell, :inline => "apt-get -y install puppet facter"
  config.vm.provision :shell, :inline => "touch /etc/puppet/hiera.yaml"
  config.vm.provision :shell, :inline => 'echo -e "mysql_root_password=puppetdrupal
controluser_password=puppetdrupal" > /etc/phpmyadmin.facts;'


  config.vm.provision :puppet do |puppet|
    puppet.manifests_path = "manifests"
    puppet.module_path = "modules"
    puppet.options = ['--verbose']
  end
  config.vm.provision :shell, :inline => "[[ ! -f /var/www/drupal/sites/default/settings.php ]] && (cd /var/www ; drush dl -y --drupal-project-rename=drupal 2>&1 >/dev/null )"
  config.vm.provision :shell, :inline => "cp -anr /var/www/drupal/sites/default/default.settings.php /var/www/drupal/sites/default/settings.php;"
  config.vm.provision :shell, :inline => "chmod a+w /var/www/drupal/sites/default ; mkdir /var/www/drupal/sites/default/files ; chown -R www-data:www-data /var/www/drupal"
  config.vm.provision :shell, :inline => "apt-get clean; rm -rf /var/www/drupal/.git"
end

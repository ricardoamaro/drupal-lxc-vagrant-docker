drupal-lxc-vagrant-docker
=========================

Deploy and develop on Drupal with LXC, Vagrant and Docker.
Installing Drupal on lxc containers has never been so easy.

## You will get:
0. Drupal (latest version) 
1. Nginx
2. Php + php-fpm
3. Mysql
4. Phpmyadmin
5. xhprof
6. xdebug
7. composer


### Clone code

```
git clone git@github.com:ricardoamaro/drupal-lxc-vagrant-docker.git
cd ~/drupal-lxc-vagrant-docker
```

### Install & Deploy

Install latest Vagrant from:
http://downloads.vagrantup.com/tags/v1.2.7 or later.

```
sudo apt-get install lxc redir
sudo vagrant plugin install vagrant-lxc
sudo vagrant up --provider=lxc drupal
sudo lxc-ls --fancy
```

### Configue Networking
```
# redirect port 80 to the host
sudo redir --lport=80 --cport=80 --caddr=$(lxc-list | grep drupal-lxc | awk '{print $3}') &
```
your /etc/hosts file should have a line like this:
```
127.0.2.1	drupal phpmyadmin xhprof
```

### Develop on Drupal
* Access Drupal on http://drupal
* Access Phpmyadmin on http://phpmyadmin/
* Access drupal files on /var/lib/lxc/{container name}/rootfs/var/www/
* Access XHProf logs at http://xhprof
* Mysql root password: puppetdrupal

#### Stop lxc container with:
```
~/drupal-lxc-vagrant-docker# vagrant stop
```

#### Destroy lxc container with:
```
~/drupal-lxc-vagrant-docker# vagrant destroy
~/drupal-lxc-vagrant-docker# killall redir
```

## DOCKER

### Import container to docker:
```
tar -C /var/lib/lxc/{container name}/rootfs/ -c . | docker import - DEV/drupal
```

### Start docker 
```
docker run -i -t -p :80 DEV:drupal /bin/bash
```

### Future ideas:
* Since this a pure devops work twoards actual running production environments,
one of the primary targets is to deploy to the cloud using several hosts to achieve a real hosting structure.
* Using the shipping container concept of docker it would be great to have the container fill up the several jobs 
that Drupal work with, like:
- Separated mysql/mariadb, 
- Redis, 
- Solr, 
- Varnish,
- Load balancers...

### Known Issues
* Upstart on Docker is neutered due to [this issue][docker_upstart_issue].
* Warning: This is still in development and ports shouldn't be open to the outside world

## Development contrib
Feel free to fork and contribute to this code. :)

1. Fork the repo
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Added some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## Authors

Created and maintained by [Ricardo Amaro][author] (<mail@ricardoamaro.com>)

## License
GPL v3

#### Special Thank you
To these projects for their awesome code:
https://github.com/fgrehm/vagrant-lxc
https://github.com/mitchellh/vagrant
https://github.com/dotcloud/docker
https://github.com/puppet/puppet
and to all other FreeSoftware used on this repo, 
including the fabulous
http://drupal.org
=================

[author]:                 https://github.com/ricardoamaro
[docker_upstart_issue]:   https://github.com/dotcloud/docker/issues/223
[docker_index]:           https://index.docker.io/

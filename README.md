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


### Deploy code

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
sudo redir --lport=80 --cport=80 --caddr={listed ip} &
```

### Configue Networking
your /etc/hosts file should have something like:
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

### Known Issues
* Upstart is neutered due to [this issue][docker_upstart_issue].
* Warning: This is still in development and ports shouldn't be open to the outside world

## Development
Feel free to fork and contribute to this code. :)

1. Fork the repo
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Added some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## License
GPL v3

#### Special Thank you
To these projects for their awesome code:
https://github.com/fgrehm/vagrant-lxc
https://github.com/mitchellh/vagrant
https://github.com/dotcloud/docker
https://github.com/puphpet/puphpet
and to all other FreeSoftware used on this repo, 
including the fabulous
http://drupal.org
=================
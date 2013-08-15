drupal-lxc-vagrant-docker
=========================

Deploy and develop on Drupal with LXC, Vagrant and Docker.
Installing Drupal on lxc containers has never been so easy.

## You will get:
0. Drupal (latest version) 
1. Nginx
2. Php 5.3 + php-fpm
3. Mysql
4. Phpmyadmin
5. xhprof
6. xdebug
7. composer


## Mysql root password
puppetdrupal

## Phpmyadmin will be on:
http://phpmyadmin

## Deploy code

git clone {this repo}

## Install & Run

Install latest Vagrant from:
http://downloads.vagrantup.com/tags/v1.2.7
or later

```
apt-get install lxc redir
vagrant plugin install vagrant-lxc
vagrant up --provider=lxc
lxc-ls --fancy
redir --lport=80 --cport=80 --caddr={listed ip} &
```

## Configue Networking
your /etc/hosts file should have:
127.0.1.1	workbox phpmyadmin drupal


## Develop 
Access Drupal on http://drupal
Access Phpmyadmin on http://phpmyadmin/
Access drupal files on /var/lib/lxc/{container name}/rootfs/var/www/


## Destroy lxc container with:


# Warning
This is still in development and ports shouldn't be open to the outside world
Feel free to fork and contribute to this code. :)



Special thank you to these projects for their awesome code:
https://github.com/fgrehm/vagrant-lxc
https://github.com/mitchellh/vagrant
https://github.com/dotcloud/docker
https://github.com/puphpet/puphpet
and to all other FreeSoftware used on this repo, including the fabulous http://drupal.org

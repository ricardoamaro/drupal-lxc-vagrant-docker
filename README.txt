drupal-lxc-vagrant-docker
=========================
#### What are these?
LXC is a lightweight virtualization method that provides operating system-level virtualization 
optional to an heavy full virtual machine. It relies on the Linux kernel cgroups functionality 
that became available in version 2.6.24, It provides a virtual environment that has its own process and network space. 
This option makes the perfect option for deploying several contained Drupal dev environments 
independent of the distribution.Docker is a solution from dotCloud, 
which simplifies and improves the process of creating and managing Linux containers.
Vagrant 1.1+ lxc plugin allows it to control and provision Linux Containers as an alternative 
to the built in (and heavy) Vagrant VirtualBox provider for Linux hosts.

Deploy and develop on Drupal with LXC, using Vagrant and/or Docker.

Takes about ~2 minutes to have a full running Drupal development box.
Installing Drupal on lxc containers has never been faster and easier.


## You will get:
0. Drupal (latest version) 
1. Nginx
2. Php + php-fpm
3. Mysql
4. Phpmyadmin
5. xhprof
6. xdebug
7. composer


### Install

Install latest Vagrant from:
http://downloads.vagrantup.com/tags/v1.2.7 or later.

```
sudo dpkg -i vagrant_1.2.7_x86_64.deb
sudo apt-get install lxc redir
```

### Clone this code

```
git clone git@github.com:ricardoamaro/drupal-lxc-vagrant-docker.git
cd ~/drupal-lxc-vagrant-docker
```

#### Get the plugin & Deploy
```
vagrant plugin install vagrant-lxc
vagrant up --provider=lxc 
sudo lxc-ls --fancy
```

### Configue Networking
```
# redirect port 80 to the host
sudo redir --lport=80 --cport=80 --caddr={container ip} &
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

#### Stop/start lxc container with:
```
~/drupal-lxc-vagrant-docker# vagrant halt
~/drupal-lxc-vagrant-docker# vagrant up --no-provision
```

#### Destroy lxc container with:
```
~/drupal-lxc-vagrant-docker# vagrant destroy
~/drupal-lxc-vagrant-docker# sudo killall redir
```

## DOCKER

Docker will be used to ship our newly created appliance, deploying it to any linux server, anywhere in the world, whitin a container.

### Install docker
```
sudo apt-get -y install docker
curl get.docker.io | sudo sh -x
```

### Import container to docker:
```
sudo tar -C /var/lib/lxc/{container name}/rootfs/ -c . | sudo docker import - dev/drupal
```

### Start docker 
```
sudo docker run -i -t -p :80 dev/drupal /bin/bash
```
### docker image

An already cooked Docker image has been commited to https://index.docker.io, and can be pulled using: 
```
sudo docker pull ricardoamaro/drupal
```

*There is also a project to build a simple lamp image with Drupal, using Dockerfile:
https://github.com/ricardoamaro/docker-drupal

*You can find more images using the [Docker Index][docker_index].

### Future ideas:
* Since this a pure devops work towards actual running production environments,
one of the primary targets is to deploy to the cloud using several hosts to achieve a real hosting structure.
* Using the shipping container concept of docker it would be great to have the container fill up the several jobs 
that Drupal work with, like:
- Separated mysql/mariadb, 
- Redis, 
- Solr, 
- Varnish,
- Load balancers...

### Known Issues
* Upstart on Docker is broken due to [this issue][docker_upstart_issue], and that's one of the reasons the image is puppetized using vagrant.
* Warning: This is still in development and ports shouldn't be open to the outside world.
* The vagrant uses the vagrant pub ssh key, but you can remove that while using `lxc-attach -n {container name}`


## Contributing
Feel free to fork and contribute to this code. :)

1. Fork the repo
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Added some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## Authors

Created and maintained by [Ricardo Amaro][author] (<mail [at] ricardoamaro. com>)

## License
GPLv2+

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

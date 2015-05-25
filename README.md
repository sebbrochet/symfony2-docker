# Symfony2 with Docker Sample

Sample Symfony2 application on Docker (production and development environment).

**NOTE:** This project requires Vagrant 1.4+.

## Installation

- Ensure you have the following tools installed on your computer:
  - Vagrant (http://vagrantup.com)
  - VirtualBox (http://www.virtualbox.org)
- Install vagrant-hostupdater plugin `vagrant plugin install vagrant-hostsupdater`
- Install vagrant-vbguest plugin `vagrant plugin install vagrant-vbguest`
- Run `vagrant up`

## Building images

- Run `vagrant ssh` to access SSH into running VM
- Go to `/vagrant` directory
- Run `./build.sh` to build all docker images or run each command yourself.

### What's inside ?

All images are based on `ubuntu:14.10`

- sample/apache
  - sample/apache-php:prod
    - sample/apache-php:dev
	  - sample/application:dev
	- sample/application:prod
	
#### sample/apache

Clean apache2 docker image.

#### sample/apache-php:prod

Image based on `sample/apache`.

Apache2 with php 5.5. Also installed `curl` and `composer`.

#### sample/apache-php:dev

Image based on `sample/apache-php:prod`.

Added `xdebug` and `webgrind`.

#### sample/application:prod

Image based on `sample/apache-php:prod`

Symfony2 application. Code is under `/srv/application/`.

#### sample/application:dev

Image based on `sample/apache-php:dev`

Image prepared to by run with mounted shared volume with application code to `/srv/application/`. 


## Production

TBD

## Development environment

Running docker image.

#### 1. Default command

Default CMD starts apache (it doesn't install composer dependencies or install assets)

Inside the VM command-line run:

`docker run -p 80:80 -v /vagrant/application/:/srv/application:rw sample/application:dev`

Now you have running instance of your application under `http://symfony2-docker-new.playground/app_dev.php`

#### 2. /bin/bash 

If you want develop your application inside docker instance, run command in the VM command-line:

`docker run -i -t -p 80:80 -v /vagrant/application/:/srv/application:rw sample/application:dev /bin/bash`

Now you should be inside docker instance. 

Next step is run apache: `service apache2 start`.

Or use PHP built-in web server: `php app/console server:run 0.0.0.0:80`

Application code is in `/srv/application/`.

## TODO

- Create docker images for databases (MySQL, NoSQL)
- Symfony2 Simple CRUDs
- Make possible to pass `TIMEZONE` to `docker build` or `docker run`
- Make possible to pas `github` to `docker build` (Required sometimes for composer ([see](https://github.com/composer/composer/issues/2366)))
# Vagrant Ansible Wordpress

![Vagrant-Ansible-Wordpress](http://i.imgur.com/0Pu2E9A.png)

This is a WordPress development environment that uses Vagant and Ansible to start developing in less than 5 minutes.
In addition to this the LAMP environment allows to serve any type of PHP application (```html``` folder).

## Prerequisites

You'll need to have the following prerequisites **installed** on your workstation:

* [VirtualBox](https://www.virtualbox.org/)
* [Vagrant](http://www.vagrantup.com/)
* [Ansible](http://www.ansibleworks.com)
* Add an appropriate vagrant box (optional)
```bash
# Defalut
vagrant box add Mayccoll/Vagrant-Ansible-Wordpress
# or
vagrant box add bento/ubuntu-16.04
```

## What is included?

- **Wordpress**
- **PHP 7**
- **Apache**
- **MySql**
- **PhpMyAdmin**
- Wp-cli
- Unzip
- Curl
- Git
- Zsh
- Oh-My-ZSH
- Terminal Fonts

## Quick Start :checkered_flag:


```bash
  $ git clone git@github.com:Mayccoll/Vagrant-Ansible-Wordpress.git
  $ cd Vagrant-Ansible-Wordpress
  $ vagrant up

  open http://192.168.70.70
```

**DONE!!!**

Once the process is finished you will see the installation data in your terminal

![Terminal](http://i.imgur.com/CFPQ59Y.png)

#### Install wordpress

In your browser go to http://192.168.70.70 and follow the installation process.


![Wordpress](http://i.imgur.com/EiatoRN.png)

**Optional:**
you can add the next rule to your host file and go to  http://mywordpress.local

```bashbash
192.168.70.70     mywordpress.local
```

use this command line to add the rule:

```bash
$ echo "\n192.168.70.70     mywordpress.local" | sudo tee -a /etc/hosts
```

-----

#### Access wordpress files:

Inside your repository in ```./www/wordpress/``` folder you will find all the wordpress files

```bash
.
├── ansible
├── config.yaml
├── README
├── html # (You can serve php files here port 8888)
├── share
├── Vagrantfile
└── www
    └── wordpress
        ├── wp-admin
        ├── wp-content
        ├── wp-includes
        ├── index.php
        ├── license.txt
        ├── readme.html
        ├── wp-activate.php
        ├── wp-blog-header.php
        ├── wp-comments-post.php
        ├── wp-config-sample.php
        ├── wp-cron.php
        ├── wp-links-opml.php
        ├── wp-load.php
        ├── wp-login.php
        ├── wp-mail.php
        ├── wp-settings.php
        ├── wp-signup.php
        ├── wp-trackback.php
        └── xmlrpc.php

```
## Fast Configuration

For fast configuration you can modify this variables in ```CONFIG.yml``` file.

```ỳaml
# |                              ↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓
local_domain : &local_domain    'mywordpress.local'
private_ip   : &private_ip      '192.168.70.70'
machine_name : &machine_name    'vag-wordpress'
machine_ram  : &machine_ram     'auto'
machine_cpu  : &machine_cpu     'auto'
# |                              ↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑
```

## Advanced Configuration

There are one configuration file ```CONFIG.yml```.

```bash
.
└── CONFIG.yml

```

#### Config VirtualBox VM

  > **./CONFIG.yaml**

```yaml
- name               : vag-wordpress
  box                : bento/ubuntu-16.04
  box_version        : 2.2.9
  box_check_update   : false
  ram                : 1028
  cpus               : 1
```

#### Defining a Forwarded Port

  > **./CONFIG.yaml**

```yaml
ports           :
  - guest       : 3000
    host        : 3000

  - guest       : 80
    host        : 8080
```

#### Config Synced folders

  > **./CONFIG.yaml**

```yaml
  syncDir         :
    - host        : share
      guest       : /home/vagrant/share

    - host        : www
      guest       : /home/vagrant/www
      owner       : vagrant
      group       : vagrant
      dmode       : 775
      fmode       : 775
```


## Ansible


#### Ansible Playbook

This playbook will install all the dependencies for wordpress.

It can be found in:

  > **./ansible/playbook.yaml**

#### Config Ansible Vars

You can config wordpress database, user and password in ```CONFIG.yml```
  > **./CONFIG.yml**


```yaml
---
  # | ······ Set Domain URL
  wpDomain          : 'mywordpress.local'

  # | ······ MySQL Config
  mysqlUser         : 'root'
  mysqlPass         : 'vagrant'
  mysqlTemplatePath : 'templates/my.cnf'

  # | ······ MySQL Database
  dbName            : 'wordpress'
  dbUser            : 'wordpress'
  dbPass            : 'vagrant'

  # | ······ Wordpress Config
  wordpressPath     : '/home/vagrant/www'
  wordpressTemPath  : 'templates/wp-config.php'
  vhostTemplatePath : 'templates/vhost.conf'

  # | ······ Vagrant User and Group
  home              : '/home/vagrant'
  owner             : 'vagrant'
  group             : 'vagrant'

```


## PhpMyAdmin

http://192.168.70.99:8088

- **Username:** wordpress
- **Password:** vagrant


## LAMP

http://192.168.70.99:8080

Local folder: ```./html```

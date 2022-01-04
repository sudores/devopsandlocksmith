---    
title: "Remine Installtion"    
date: 2022-01-03T23:11:04+02:00    
draft: false
tags: [kb,redmine,setup]
categories: [kb,setup]
---    

REDMINE INSTALLATION UBUNTU 18.04
https://kifarunix.com/install-redmine-with-mariadb-on-debian-10-buster/

Install Mariadb
```bash
  apt install mariadb-server-10.1/bionic-updates mariadb-server-core-10.1/bionic-updates mariadb-client-core-10.1/bionic-updates mariadb-client-10.1/bionic-updates mariadb-server/bionic-updates
 ```

Install Necessary deps
 ```bash
  apt install pkg-config libmariadb-dev rmagik imagemagick build-essential ruby-dev libxslt1-dev libmariadb-dev libxml2-dev zlib1g-dev imagemagick libmagickwand-dev apache2 libmariadbclient-dev libapache2-mod-passenger/bionic-updates
```
Donwload redmine 
```bash
  wget https://www.redmine.org/releases/redmine-4.0.9.tar.gz
```
Unarchive redmine
```bash
  tar -xzvf redmine-4.0.9.tar.gz 
```
Move redmine files to /opt
```bash
  mv redmine-4.0.9 /opt/redmine
```

Edit /opt/redmine/config/database.yaml
``` yaml
  production:
    adapter: mysql2
    database: redmine
    host: localhost
    username: redmine
    password: "SecurePass"
    encoding: utf8
```

Start Mariadb
```bash
	systemctl enable --now mysql
```

Run secure installation
```bash
  mysql_secure_installation
```

Create and setup DB
```bash
  mysql -u root -p 
    CREATE DATABASE redmine DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
    grant all on *.* to redmine@'%' identified by "SecurePass";
	flush privileges;
```
Setup ruby deps
```bash
  bundle install # in redmine dir
```

Setup db for redmine
```bash
  # create tables
  RAILS_ENV=production bundle exec rake db:migrate
  # Load default data
  RAILS_ENV=production REDMINE_LANG=en bundle exec rake redmine:load_default_data
```

Create apache2 redmine config
```xml 
  Listen 3000
  <VirtualHost *:3000>
          ServerName redmine.kifarunix-demo.com
          RailsEnv production
          DocumentRoot /opt/redmine/public

          <Directory "/opt/redmine/public">
                  Allow from all
                  Require all granted
          </Directory>

          ErrorLog ${APACHE_LOG_DIR}/redmine_error.log
          CustomLog ${APACHE_LOG_DIR}/redmine_access.log combined
  </VirtualHost>
```

Enable Apache2 redmine module

```bash
  a2enmod passenger
```
Check if passenger module enable

```bash
  apache2ctl -M | grep -i passenger
```

Reload Apache2 
```bash
  systemctl reload apache2
```
Enable Apache2 redmine config
```bash
  a2ensite redmine
```

Login to redmine as admin
  http://<you-domanin>:3000/
  

# MacBook 安装开发环境

## Homebrew 

## 安装

```shell
# https://brew.sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```



## 更换镜像源

```shell
# https://www.giserdqy.com/os/macos/homebrew/37220/

# 替换brew.git:
 cd "$(brew --repo)"
 git remote set-url origin https://mirrors.aliyun.com/homebrew/brew.git
 # 替换homebrew-core.git:
 cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
 git remote set-url origin https://mirrors.aliyun.com/homebrew/homebrew-core.git
 # 应用生效
 brew update
 # 替换homebrew-bottles:
 echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.aliyun.com/homebrew/homebrew-bottles' >> ~/.bash_profile
source ~/.bash_profile
```



### PHP

1. PHP 8.0

   ```SHELL
   % brew install php@8.0
   ........
   ........
   ........
   ........
   To enable PHP in Apache add the following to httpd.conf and restart Apache:
       LoadModule php_module /usr/local/opt/php@8.0/lib/httpd/modules/libphp.so
   
       <FilesMatch \.php$>
           SetHandler application/x-httpd-php
       </FilesMatch>
   
   Finally, check DirectoryIndex includes index.php
       DirectoryIndex index.php index.html
   
   The php.ini and php-fpm.ini file can be found in:
       /usr/local/etc/php/8.0/
   
   php@8.0 is keg-only, which means it was not symlinked into /usr/local,
   because this is an alternate version of another formula.
   
   If you need to have php@8.0 first in your PATH, run:
     echo 'export PATH="/usr/local/opt/php@8.0/bin:$PATH"' >> ~/.zshrc
     echo 'export PATH="/usr/local/opt/php@8.0/sbin:$PATH"' >> ~/.zshrc
   
   For compilers to find php@8.0 you may need to set:
     export LDFLAGS="-L/usr/local/opt/php@8.0/lib"
     export CPPFLAGS="-I/usr/local/opt/php@8.0/include"
   
   
   To restart php@8.0 after an upgrade:
     brew services restart php@8.0
   Or, if you don't want/need a background service you can just run:
     /usr/local/opt/php@8.0/sbin/php-fpm --nodaemonize
   ==> Summary
   🍺  /usr/local/Cellar/php@8.0/8.0.20: 500 files, 77.9MB
   ==> Running `brew cleanup php@8.0`...
   Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
   Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
   ==> Caveats
   ==> php@8.0
   To enable PHP in Apache add the following to httpd.conf and restart Apache:
       LoadModule php_module /usr/local/opt/php@8.0/lib/httpd/modules/libphp.so
   
       <FilesMatch \.php$>
           SetHandler application/x-httpd-php
       </FilesMatch>
   
   Finally, check DirectoryIndex includes index.php
       DirectoryIndex index.php index.html
   
   The php.ini and php-fpm.ini file can be found in:
       /usr/local/etc/php/8.0/
   
   php@8.0 is keg-only, which means it was not symlinked into /usr/local,
   because this is an alternate version of another formula.
   
   If you need to have php@8.0 first in your PATH, run:
     echo 'export PATH="/usr/local/opt/php@8.0/bin:$PATH"' >> ~/.zshrc
     echo 'export PATH="/usr/local/opt/php@8.0/sbin:$PATH"' >> ~/.zshrc
   
   For compilers to find php@8.0 you may need to set:
     export LDFLAGS="-L/usr/local/opt/php@8.0/lib"
     export CPPFLAGS="-I/usr/local/opt/php@8.0/include"
   
   
   To restart php@8.0 after an upgrade:
     brew services restart php@8.0
   Or, if you don't want/need a background service you can just run:
     /usr/local/opt/php@8.0/sbin/php-fpm --nodaemonize
   shaogaojie@192 projects %
   shaogaojie@192 projects % brew update
   
   ```

   

2. PHP 5.6

3. 



### NGINX

```shell
brew install nginx

==> Downloading https://ghcr.io/v2/homebrew/core/nginx/manifests/1.23.0
==> Downloading https://ghcr.io/v2/homebrew/core/nginx/blobs/sha256:53dbe53fea64c8482d8321ff6ef61c2d323d1db0189e82a1e3f25a0cd6ebf3ea
==> Downloading from https://pkg-containers.githubusercontent.com/ghcr1/blobs/sha256:53dbe53fea64c8482d8321ff6ef61c2d323d1db0189e82a1e3f25a0cd6ebf3ea?se=2022-06-29T03%3A45%3A00Z&sig=G2YMEO%2FepA1rrNUwDJ93JIIqcB6njfmQZEjzJjcgT2o%3D&sp=r&spr=https&sr=b&sv=2019-12-12
==> Pouring nginx--1.23.0.monterey.bottle.tar.gz
==> Caveats
Docroot is: /usr/local/var/www

The default port has been set in /usr/local/etc/nginx/nginx.conf to 8080 so that
nginx can run without sudo.

nginx will load all files in /usr/local/etc/nginx/servers/.

To restart nginx after an upgrade:
  brew services restart nginx
Or, if you don't want/need a background service you can just run:
  /usr/local/opt/nginx/bin/nginx -g daemon off;
==> Summary
🍺  /usr/local/Cellar/nginx/1.23.0: 26 files, 2.2MB
==> Running `brew cleanup nginx`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
任务完成: (0) at 2022/6/29 11:37!
```



### MYSQL

```shell
brew install mysql
==> Downloading https://mirrors.aliyun.com/homebrew/homebrew-bottles/libevent-2.1.12.monterey.bottle
######################################################################## 100.0%
........
........
........
==> Installing dependencies for mysql: libevent, libcbor, libfido2, lz4 and protobuf
==> Installing mysql dependency: libevent
==> Pouring libevent-2.1.12.monterey.bottle.tar.gz
🍺  /usr/local/Cellar/libevent/2.1.12: 57 files, 2.0MB
==> Installing mysql dependency: libcbor
==> Pouring libcbor-0.9.0.monterey.bottle.tar.gz
🍺  /usr/local/Cellar/libcbor/0.9.0: 31 files, 161.6KB
==> Installing mysql dependency: libfido2
==> Pouring libfido2-1.11.0.monterey.bottle.tar.gz
🍺  /usr/local/Cellar/libfido2/1.11.0: 512 files, 1MB
==> Installing mysql dependency: lz4
==> Pouring lz4-1.9.3.monterey.bottle.tar.gz
🍺  /usr/local/Cellar/lz4/1.9.3: 22 files, 647.7KB
==> Installing mysql dependency: protobuf
==> Pouring protobuf-3.19.4.monterey.bottle.tar.gz
🍺  /usr/local/Cellar/protobuf/3.19.4: 270 files, 19.6MB
==> Installing mysql
==> Pouring mysql-8.0.29.monterey.bottle.tar.gz
==> /usr/local/Cellar/mysql/8.0.29/bin/mysqld --initialize-insecure --user=shaogaojie --basedir=/usr
==> Caveats
We've installed your MySQL database without a root password. To secure it run:
    mysql_secure_installation

MySQL is configured to only allow connections from localhost by default

To connect run:
    mysql -uroot

To restart mysql after an upgrade:
  brew services restart mysql
Or, if you don't want/need a background service you can just run:
  /usr/local/opt/mysql/bin/mysqld_safe --datadir=/usr/local/var/mysql
==> Summary
🍺  /usr/local/Cellar/mysql/8.0.29: 311 files, 294.7MB
==> Running `brew cleanup mysql`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
==> Caveats
==> mysql
We've installed your MySQL database without a root password. To secure it run:
    mysql_secure_installation

MySQL is configured to only allow connections from localhost by default

To connect run:
    mysql -uroot

To restart mysql after an upgrade:
  brew services restart mysql
Or, if you don't want/need a background service you can just run:
  /usr/local/opt/mysql/bin/mysqld_safe --datadir=/usr/local/var/mysql
shaogaojie@192 projects %


#启动服务
# brew services restart mysql
==> Successfully started `mysql` (label: homebrew.mxcl.mysql)

# 修改密码
shaogaojie@192 projects % mysql_secure_installation

Securing the MySQL server deployment.

Connecting to MySQL using a blank password.

VALIDATE PASSWORD COMPONENT can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD component?

Press y|Y for Yes, any other key for No: y

There are three levels of password validation policy:

LOW    Length >= 8
MEDIUM Length >= 8, numeric, mixed case, and special characters
STRONG Length >= 8, numeric, mixed case, special characters and dictionary                  file

Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 0
Please set the password for root here.

New password:

Re-enter new password:

Estimated strength of the password: 50
Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : y
By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them. This is intended only for
testing, and to make the installation go a bit smoother.
You should remove them before moving into a production
environment.

Remove anonymous users? (Press y|Y for Yes, any other key for No) : n

 ... skipping.


Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network.

Disallow root login remotely? (Press y|Y for Yes, any other key for No) : n

 ... skipping.
By default, MySQL comes with a database named 'test' that
anyone can access. This is also intended only for testing,
and should be removed before moving into a production
environment.


Remove test database and access to it? (Press y|Y for Yes, any other key for No) : n

 ... skipping.
Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.

Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
Success.

All done!
shaogaojie@192 projects %
shaogaojie@192 projects % mysql -uroot
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)
shaogaojie@192 projects % mysql -uroot -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 11
Server version: 8.0.29 Homebrew

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.01 sec)


```



### MONGODB

```shell
#https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-os-x/

brew tap mongodb/brew

brew install mongodb-community@5.0

==> Downloading https://fastdl.mongodb.org/tools/db/mongodb-database-tools-macos-x86_64-100.5.2.zip
######################################################################## 100.0%
==> Downloading https://mirrors.aliyun.com/homebrew/homebrew-bottles/c-ares-1.18.1_1.monterey.bottle
######################################################################## 100.0%
==> Downloading https://mirrors.aliyun.com/homebrew/homebrew-bottles/libuv-1.44.1_1.monterey.bottle.
######################################################################## 100.0%
==> Downloading https://mirrors.aliyun.com/homebrew/homebrew-bottles/node%4016-16.15.1.monterey.bott
######################################################################## 100.0%
==> Downloading https://mirrors.aliyun.com/homebrew/homebrew-bottles/mongosh-1.5.0.monterey.bottle.t
######################################################################## 100.0%
==> Downloading https://fastdl.mongodb.org/osx/mongodb-macos-x86_64-5.0.7.tgz
######################################################################## 100.0%
==> Installing mongodb-community from mongodb/brew
==> Installing dependencies for mongodb/brew/mongodb-community: mongodb-database-tools, c-ares, libuv, node@16 and mongosh
==> Installing mongodb/brew/mongodb-community dependency: mongodb-database-tools
🍺  /usr/local/Cellar/mongodb-database-tools/100.5.2: 13 files, 115.7MB, built in 3 seconds
==> Installing mongodb/brew/mongodb-community dependency: c-ares
==> Pouring c-ares-1.18.1_1.monterey.bottle.tar.gz
🍺  /usr/local/Cellar/c-ares/1.18.1_1: 87 files, 645.1KB
==> Installing mongodb/brew/mongodb-community dependency: libuv
==> Pouring libuv-1.44.1_1.monterey.bottle.tar.gz
🍺  /usr/local/Cellar/libuv/1.44.1_1: 49 files, 3.5MB
==> Installing mongodb/brew/mongodb-community dependency: node@16
==> Pouring node@16-16.15.1.monterey.bottle.tar.gz
🍺  /usr/local/Cellar/node@16/16.15.1: 1,850 files, 47.9MB
==> Installing mongodb/brew/mongodb-community dependency: mongosh
==> Pouring mongosh-1.5.0.monterey.bottle.tar.gz
🍺  /usr/local/Cellar/mongosh/1.5.0: 5,931 files, 33.5MB
==> Installing mongodb/brew/mongodb-community
==> Caveats
To start mongodb/brew/mongodb-community now and restart at login:
  brew services start mongodb/brew/mongodb-community
Or, if you don't want/need a background service you can just run:
  mongod --config /usr/local/etc/mongod.conf
==> Summary
🍺  /usr/local/Cellar/mongodb-community/5.0.7: 11 files, 182.5MB, built in 4 seconds
==> Running `brew cleanup mongodb-community`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
==> Caveats
==> mongodb-community
To start mongodb/brew/mongodb-community now and restart at login:
  brew services start mongodb/brew/mongodb-community
Or, if you don't want/need a background service you can just run:
  mongod --config /usr/local/etc/mongod.conf
shaogaojie@192 projects %
shaogaojie@192 projects %
shaogaojie@192 projects %
shaogaojie@192 projects %

```



### REDIS

```shell
brew install redis
==> Downloading https://mirrors.aliyun.com/homebrew/homebrew-bottles/redis-7.0.2.monterey.bottle.tar
######################################################################## 100.0%
==> Pouring redis-7.0.2.monterey.bottle.tar.gz
==> Caveats
To restart redis after an upgrade:
  brew services restart redis
Or, if you don't want/need a background service you can just run:
  /usr/local/opt/redis/bin/redis-server /usr/local/etc/redis.conf
==> Summary
🍺  /usr/local/Cellar/redis/7.0.2: 14 files, 2.6MB
==> Running `brew cleanup redis`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
```






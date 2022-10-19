# ClashX

## 配置规则

```shell
cd ~/.config/clash/

vim proxyIgnoreList.plist

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<array>
        <string>192.168.0.0/16</string>
        <string>10.0.0.0/8</string>
        <string>172.16.0.0/12</string>
        <string>127.0.0.1</string>
        <string>localhost</string>
        <string>*.local</string>
        <string>*.crashlytics.com</string>
        <string>*.xxxxxx.com</string> # 如果想把某个域名加入白名单 不走vpn 可以在这里添加 然后重启ClashX即可。
</array>
</plist>
```



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

#### PHP 8.0

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

##### Redis.so 扩展



```shell
git clone https://www.github.com/phpredis/phpredis.git
cd phpredis
phpize && ./configure && make && sudo make install


Password:
Installing shared extensions:     /usr/local/Cellar/php@8.0/8.0.20/pecl/20200930/
shaogaojie@192 phpredis % php --ini
Configuration File (php.ini) Path: /usr/local/etc/php/8.0
Loaded Configuration File:         /usr/local/etc/php/8.0/php.ini
Scan for additional .ini files in: /usr/local/etc/php/8.0/conf.d
Additional .ini files parsed:      /usr/local/etc/php/8.0/conf.d/ext-opcache.ini

shaogaojie@192 phpredis % cd /usr/local/Cellar/php@8.0/8.0.20/pecl/20200930/


extension=/usr/local/Cellar/php@8.0/8.0.20/pecl/20200930/redis.so

重启服务即可。

```

##### 常见问题

1. 31481#0: *212 open() "/usr/local/var/run/nginx/client_body_temp/0000000050" failed (13: Permission denied)

   ```shell
   https://github.com/denji/homebrew-nginx/issues/124
   
   ~ sudo chown -vhR $USER:admin /usr/local/var/run/nginx/*_temp
   ~ sudo chmod -vR 700 /usr/local/var/run/nginx/*_temp
   重启Nginx
   ```

   

2. 

#### PHP7.3

```shell
brew install shivammathur/php/php@7.3

==> Caveats
To enable PHP in Apache add the following to httpd.conf and restart Apache:
    LoadModule php7_module /usr/local/opt/php@7.3/lib/httpd/modules/libphp7.so

    <FilesMatch \.php$>
        SetHandler application/x-httpd-php
    </FilesMatch>

Finally, check DirectoryIndex includes index.php
    DirectoryIndex index.php index.html

The php.ini and php-fpm.ini file can be found in:
    /usr/local/etc/php/7.3/

php@7.3 is keg-only, which means it was not symlinked into /usr/local,
because this is an alternate version of another formula.

If you need to have php@7.3 first in your PATH, run:
  echo 'export PATH="/usr/local/opt/php@7.3/bin:$PATH"' >> ~/.zshrc
  echo 'export PATH="/usr/local/opt/php@7.3/sbin:$PATH"' >> ~/.zshrc

For compilers to find php@7.3 you may need to set:
  export LDFLAGS="-L/usr/local/opt/php@7.3/lib"
  export CPPFLAGS="-I/usr/local/opt/php@7.3/include"


To restart shivammathur/php/php@7.3 after an upgrade:
  brew services restart shivammathur/php/php@7.3
Or, if you don't want/need a background service you can just run:
  /usr/local/opt/php@7.3/sbin/php-fpm --nodaemonize
==> Summary
🍺  /usr/local/Cellar/php@7.3/7.3.33_2: 518 files, 74.7MB
==> Running `brew cleanup php@7.3`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
==> Caveats
==> php@7.3
To enable PHP in Apache add the following to httpd.conf and restart Apache:
    LoadModule php7_module /usr/local/opt/php@7.3/lib/httpd/modules/libphp7.so

    <FilesMatch \.php$>
        SetHandler application/x-httpd-php
    </FilesMatch>

Finally, check DirectoryIndex includes index.php
    DirectoryIndex index.php index.html

The php.ini and php-fpm.ini file can be found in:
    /usr/local/etc/php/7.3/

php@7.3 is keg-only, which means it was not symlinked into /usr/local,
because this is an alternate version of another formula.

If you need to have php@7.3 first in your PATH, run:
  echo 'export PATH="/usr/local/opt/php@7.3/bin:$PATH"' >> ~/.zshrc
  echo 'export PATH="/usr/local/opt/php@7.3/sbin:$PATH"' >> ~/.zshrc

For compilers to find php@7.3 you may need to set:
  export LDFLAGS="-L/usr/local/opt/php@7.3/lib"
  export CPPFLAGS="-I/usr/local/opt/php@7.3/include"


To restart shivammathur/php/php@7.3 after an upgrade:
  brew services restart shivammathur/php/php@7.3
Or, if you don't want/need a background service you can just run:
  /usr/local/opt/php@7.3/sbin/php-fpm --nodaemonize
  
  
修改端口：
vim /usr/local/etc/php/7.3/php-fpm.d/www.conf

listen = 127.0.0.1:9073 # 修改为9073端口
```



#### PHP 5.6

##### PHP5.6 

```shell
https://stackoverflow.com/questions/54143760/how-to-install-php-5-6-with-homebrew-if-from-this-year-it-is-eol

brew tap shivammathur/php
brew install shivammathur/php/php@5.6

Warning: shivammathur/php/php@5.6 has been deprecated because it is deprecated upstream!

To enable PHP in Apache add the following to httpd.conf and restart Apache:
    LoadModule php5_module /usr/local/opt/php@5.6/lib/httpd/modules/libphp5.so

    <FilesMatch \.php$>
        SetHandler application/x-httpd-php
    </FilesMatch>

Finally, check DirectoryIndex includes index.php
    DirectoryIndex index.php index.html

The php.ini and php-fpm.ini file can be found in:
    /usr/local/etc/php/5.6/

php@5.6 is keg-only, which means it was not symlinked into /usr/local,
because this is an alternate version of another formula.

If you need to have php@5.6 first in your PATH, run:
  echo 'export PATH="/usr/local/opt/php@5.6/bin:$PATH"' >> ~/.zshrc
  echo 'export PATH="/usr/local/opt/php@5.6/sbin:$PATH"' >> ~/.zshrc

For compilers to find php@5.6 you may need to set:
  export LDFLAGS="-L/usr/local/opt/php@5.6/lib"
  export CPPFLAGS="-I/usr/local/opt/php@5.6/include"


To restart shivammathur/php/php@5.6 after an upgrade:
  brew services restart shivammathur/php/php@5.6
Or, if you don't want/need a background service you can just run:
  /usr/local/opt/php@5.6/sbin/php-fpm --nodaemonize
==> Summary
🍺  /usr/local/Cellar/php@5.6/5.6.40_4: 499 files, 60.4MB
==> Running `brew cleanup php@5.6`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
==> Caveats
==> php@5.6
To enable PHP in Apache add the following to httpd.conf and restart Apache:
    LoadModule php5_module /usr/local/opt/php@5.6/lib/httpd/modules/libphp5.so

    <FilesMatch \.php$>
        SetHandler application/x-httpd-php
    </FilesMatch>

Finally, check DirectoryIndex includes index.php
    DirectoryIndex index.php index.html

The php.ini and php-fpm.ini file can be found in:
    /usr/local/etc/php/5.6/

php@5.6 is keg-only, which means it was not symlinked into /usr/local,
because this is an alternate version of another formula.

If you need to have php@5.6 first in your PATH, run:
  echo 'export PATH="/usr/local/opt/php@5.6/bin:$PATH"' >> ~/.zshrc
  echo 'export PATH="/usr/local/opt/php@5.6/sbin:$PATH"' >> ~/.zshrc

For compilers to find php@5.6 you may need to set:
  export LDFLAGS="-L/usr/local/opt/php@5.6/lib"
  export CPPFLAGS="-I/usr/local/opt/php@5.6/include"


To restart shivammathur/php/php@5.6 after an upgrade:
  brew services restart shivammathur/php/php@5.6
Or, if you don't want/need a background service you can just run:
  /usr/local/opt/php@5.6/sbin/php-fpm --nodaemonize
  
```



##### MEMCACHE.SO

```shell
https://pecl.php.net/package/memcache
# 进入目录
cd memcache-3.0.8
shaogaojie@shaogaojiedeMacBook-Pro memcache-3.0.8 % ls -l
total 64
drwxr-xr-x@ 23 shaogaojie  staff    736  6 30 11:03 memcache-3.0.8
-rw-rw-r--@  1 shaogaojie  staff  28745  4  8  2013 package.xml
shaogaojie@shaogaojiedeMacBook-Pro memcache-3.0.8 % cd memcache-3.0.8
shaogaojie@shaogaojiedeMacBook-Pro memcache-3.0.8 % ls -l
total 552
-rw-r--r--@  1 shaogaojie  staff     32  4  8  2013 CREDITS
-rw-r--r--@  1 shaogaojie  staff   3208  4  8  2013 LICENSE
-rw-r--r--@  1 shaogaojie  staff   5381  4  8  2013 README
-rw-r--r--@  1 shaogaojie  staff    116  4  8  2013 config.m4
-rw-r--r--@  1 shaogaojie  staff    668  4  8  2013 config.w32
-rw-r--r--@  1 shaogaojie  staff   3886  4  8  2013 config9.m4
-rw-r--r--@  1 shaogaojie  staff    509  4  8  2013 example.php
-rw-r--r--@  1 shaogaojie  staff  60936  4  8  2013 memcache.c
-rw-r--r--@  1 shaogaojie  staff   4769  4  8  2013 memcache.dsp
-rw-r--r--@  1 shaogaojie  staff  29110  4  8  2013 memcache.php
-rw-r--r--@  1 shaogaojie  staff  13608  4  8  2013 memcache_ascii_protocol.c
-rw-r--r--@  1 shaogaojie  staff  19176  4  8  2013 memcache_binary_protocol.c
-rw-r--r--@  1 shaogaojie  staff   5323  4  8  2013 memcache_consistent_hash.c
-rw-r--r--@  1 shaogaojie  staff  48821  4  8  2013 memcache_pool.c
-rw-r--r--@  1 shaogaojie  staff  17956  4  8  2013 memcache_pool.h
-rw-r--r--@  1 shaogaojie  staff   3580  4  8  2013 memcache_queue.c
-rw-r--r--@  1 shaogaojie  staff   2485  4  8  2013 memcache_queue.h
-rw-r--r--@  1 shaogaojie  staff  15998  4  8  2013 memcache_session.c
-rw-r--r--@  1 shaogaojie  staff   3101  4  8  2013 memcache_standard_hash.c
-rw-r--r--@  1 shaogaojie  staff   3988  4  8  2013 php_memcache.h
shaogaojie@shaogaojiedeMacBook-Pro memcache-3.0.8 % 

# 执行 phpize 报错啦。。。
/usr/local/opt/php@5.6/bin/phpize
Configuring for:
PHP Api Version:         20131106
Zend Module Api No:      20131226
Zend Extension Api No:   220131226
configure.in:34: warning: The macro `AC_TRY_LINK' is obsolete.
configure.in:34: You should run autoupdate.
./lib/autoconf/general.m4:2920: AC_TRY_LINK is expanded from...
lib/m4sugar/m4sh.m4:692: _AS_IF_ELSE is expanded from...
lib/m4sugar/m4sh.m4:699: AS_IF is expanded from...
./lib/autoconf/general.m4:2249: AC_CACHE_VAL is expanded from...
aclocal.m4:301: PHP_RUNPATH_SWITCH is expanded from...
configure.in:34: the top level
configure.in:149: warning: The macro `AC_LANG_C' is obsolete.
configure.in:149: You should run autoupdate.
./lib/autoconf/c.m4:72: AC_LANG_C is expanded from...
aclocal.m4:5773: _LT_AC_LANG_C_CONFIG is expanded from...
aclocal.m4:5772: AC_LIBTOOL_LANG_C_CONFIG is expanded from...
aclocal.m4:3112: AC_LIBTOOL_SETUP is expanded from...
aclocal.m4:3094: _AC_PROG_LIBTOOL is expanded from...
aclocal.m4:3081: AC_PROG_LIBTOOL is expanded from...
configure.in:149: the top level
configure.in:149: warning: The macro `AC_LANG_C' is obsolete.
configure.in:149: You should run autoupdate.
./lib/autoconf/c.m4:72: AC_LANG_C is expanded from...
lib/m4sugar/m4sh.m4:692: _AS_IF_ELSE is expanded from...
lib/m4sugar/m4sh.m4:699: AS_IF is expanded from...
./lib/autoconf/general.m4:2249: AC_CACHE_VAL is expanded from...
./lib/autoconf/general.m4:2270: AC_CACHE_CHECK is expanded from...
aclocal.m4:3603: _LT_AC_LOCK is expanded from...
aclocal.m4:4230: AC_LIBTOOL_SYS_HARD_LINK_LOCKS is expanded from...
aclocal.m4:5773: _LT_AC_LANG_C_CONFIG is expanded from...
aclocal.m4:5772: AC_LIBTOOL_LANG_C_CONFIG is expanded from...
aclocal.m4:3112: AC_LIBTOOL_SETUP is expanded from...
aclocal.m4:3094: _AC_PROG_LIBTOOL is expanded from...
aclocal.m4:3081: AC_PROG_LIBTOOL is expanded from...
configure.in:149: the top level
configure.in:149: warning: The macro `AC_TRY_LINK' is obsolete.
configure.in:149: You should run autoupdate.
./lib/autoconf/general.m4:2920: AC_TRY_LINK is expanded from...
lib/m4sugar/m4sh.m4:692: _AS_IF_ELSE is expanded from...
lib/m4sugar/m4sh.m4:699: AS_IF is expanded from...
./lib/autoconf/general.m4:2249: AC_CACHE_VAL is expanded from...
./lib/autoconf/general.m4:2270: AC_CACHE_CHECK is expanded from...
aclocal.m4:3603: _LT_AC_LOCK is expanded from...
aclocal.m4:4230: AC_LIBTOOL_SYS_HARD_LINK_LOCKS is expanded from...
aclocal.m4:5773: _LT_AC_LANG_C_CONFIG is expanded from...
aclocal.m4:5772: AC_LIBTOOL_LANG_C_CONFIG is expanded from...
aclocal.m4:3112: AC_LIBTOOL_SETUP is expanded from...
aclocal.m4:3094: _AC_PROG_LIBTOOL is expanded from...
aclocal.m4:3081: AC_PROG_LIBTOOL is expanded from...
configure.in:149: the top level
configure.in:149: warning: back quotes and double quotes must not be escaped in: $as_me:${as_lineno-$LINENO}: WARNING: \`$CC' does not support \`-c -o', so \`make -j' may be unsafe
aclocal.m4:4230: AC_LIBTOOL_SYS_HARD_LINK_LOCKS is expanded from...
aclocal.m4:5773: _LT_AC_LANG_C_CONFIG is expanded from...
aclocal.m4:5772: AC_LIBTOOL_LANG_C_CONFIG is expanded from...
aclocal.m4:3112: AC_LIBTOOL_SETUP is expanded from...
aclocal.m4:3094: _AC_PROG_LIBTOOL is expanded from...
aclocal.m4:3081: AC_PROG_LIBTOOL is expanded from...
configure.in:149: the top level
configure.in:149: warning: back quotes and double quotes must not be escaped in: $as_me: WARNING: \`$CC' does not support \`-c -o', so \`make -j' may be unsafe
aclocal.m4:4230: AC_LIBTOOL_SYS_HARD_LINK_LOCKS is expanded from...
aclocal.m4:5773: _LT_AC_LANG_C_CONFIG is expanded from...
aclocal.m4:5772: AC_LIBTOOL_LANG_C_CONFIG is expanded from...
aclocal.m4:3112: AC_LIBTOOL_SETUP is expanded from...
aclocal.m4:3094: _AC_PROG_LIBTOOL is expanded from...
aclocal.m4:3081: AC_PROG_LIBTOOL is expanded from...
configure.in:149: the top level
configure.in:149: warning: back quotes and double quotes must not be escaped in: $as_me:${as_lineno-$LINENO}: WARNING: output file \`$ofile' does not exist
aclocal.m4:4963: _LT_AC_TAGCONFIG is expanded from...
aclocal.m4:3112: AC_LIBTOOL_SETUP is expanded from...
aclocal.m4:3094: _AC_PROG_LIBTOOL is expanded from...
aclocal.m4:3081: AC_PROG_LIBTOOL is expanded from...
configure.in:149: the top level
configure.in:149: warning: back quotes and double quotes must not be escaped in: $as_me: WARNING: output file \`$ofile' does not exist
aclocal.m4:4963: _LT_AC_TAGCONFIG is expanded from...
aclocal.m4:3112: AC_LIBTOOL_SETUP is expanded from...
aclocal.m4:3094: _AC_PROG_LIBTOOL is expanded from...
aclocal.m4:3081: AC_PROG_LIBTOOL is expanded from...
configure.in:149: the top level
configure.in:149: warning: back quotes and double quotes must not be escaped in: $as_me:${as_lineno-$LINENO}: WARNING: output file \`$ofile' does not look like a libtool script
aclocal.m4:4963: _LT_AC_TAGCONFIG is expanded from...
aclocal.m4:3112: AC_LIBTOOL_SETUP is expanded from...
aclocal.m4:3094: _AC_PROG_LIBTOOL is expanded from...
aclocal.m4:3081: AC_PROG_LIBTOOL is expanded from...
configure.in:149: the top level
configure.in:149: warning: back quotes and double quotes must not be escaped in: $as_me: WARNING: output file \`$ofile' does not look like a libtool script
aclocal.m4:4963: _LT_AC_TAGCONFIG is expanded from...
aclocal.m4:3112: AC_LIBTOOL_SETUP is expanded from...
aclocal.m4:3094: _AC_PROG_LIBTOOL is expanded from...
aclocal.m4:3081: AC_PROG_LIBTOOL is expanded from...
configure.in:149: the top level
configure.in:149: warning: back quotes and double quotes must not be escaped in: $as_me:${as_lineno-$LINENO}: WARNING: using \`LTCC=$LTCC', extracted from \`$ofile'
aclocal.m4:4963: _LT_AC_TAGCONFIG is expanded from...
aclocal.m4:3112: AC_LIBTOOL_SETUP is expanded from...
aclocal.m4:3094: _AC_PROG_LIBTOOL is expanded from...
aclocal.m4:3081: AC_PROG_LIBTOOL is expanded from...
configure.in:149: the top level
configure.in:149: warning: back quotes and double quotes must not be escaped in: $as_me: WARNING: using \`LTCC=$LTCC', extracted from \`$ofile'
aclocal.m4:4963: _LT_AC_TAGCONFIG is expanded from...
aclocal.m4:3112: AC_LIBTOOL_SETUP is expanded from...
aclocal.m4:3094: _AC_PROG_LIBTOOL is expanded from...
aclocal.m4:3081: AC_PROG_LIBTOOL is expanded from...
configure.in:149: the top level
configure.in:149: warning: back quotes and double quotes must not be escaped in: tag name \"$tagname\" already exists
aclocal.m4:4963: _LT_AC_TAGCONFIG is expanded from...
aclocal.m4:3112: AC_LIBTOOL_SETUP is expanded from...
aclocal.m4:3094: _AC_PROG_LIBTOOL is expanded from...
aclocal.m4:3081: AC_PROG_LIBTOOL is expanded from...
configure.in:149: the top level
configure.in:149: warning: The macro `AC_LANG_CPLUSPLUS' is obsolete.
configure.in:149: You should run autoupdate.
./lib/autoconf/c.m4:262: AC_LANG_CPLUSPLUS is expanded from...
aclocal.m4:5855: _LT_AC_LANG_CXX_CONFIG is expanded from...
aclocal.m4:5854: AC_LIBTOOL_LANG_CXX_CONFIG is expanded from...
aclocal.m4:4963: _LT_AC_TAGCONFIG is expanded from...
aclocal.m4:3112: AC_LIBTOOL_SETUP is expanded from...
aclocal.m4:3094: _AC_PROG_LIBTOOL is expanded from...
aclocal.m4:3081: AC_PROG_LIBTOOL is expanded from...
configure.in:149: the top level
configure.in:149: warning: back quotes and double quotes must not be escaped in: $as_me:${as_lineno-$LINENO}: WARNING: \`$CC' does not support \`-c -o', so \`make -j' may be unsafe
aclocal.m4:4230: AC_LIBTOOL_SYS_HARD_LINK_LOCKS is expanded from...
aclocal.m4:5855: _LT_AC_LANG_CXX_CONFIG is expanded from...
aclocal.m4:5854: AC_LIBTOOL_LANG_CXX_CONFIG is expanded from...
aclocal.m4:4963: _LT_AC_TAGCONFIG is expanded from...
aclocal.m4:3112: AC_LIBTOOL_SETUP is expanded from...
aclocal.m4:3094: _AC_PROG_LIBTOOL is expanded from...
aclocal.m4:3081: AC_PROG_LIBTOOL is expanded from...
configure.in:149: the top level
configure.in:149: warning: back quotes and double quotes must not be escaped in: $as_me: WARNING: \`$CC' does not support \`-c -o', so \`make -j' may be unsafe
aclocal.m4:4230: AC_LIBTOOL_SYS_HARD_LINK_LOCKS is expanded from...
aclocal.m4:5855: _LT_AC_LANG_CXX_CONFIG is expanded from...
aclocal.m4:5854: AC_LIBTOOL_LANG_CXX_CONFIG is expanded from...
aclocal.m4:4963: _LT_AC_TAGCONFIG is expanded from...
aclocal.m4:3112: AC_LIBTOOL_SETUP is expanded from...
aclocal.m4:3094: _AC_PROG_LIBTOOL is expanded from...
aclocal.m4:3081: AC_PROG_LIBTOOL is expanded from...
configure.in:149: the top level
configure.in:200: warning: The macro `AC_CONFIG_HEADER' is obsolete.
configure.in:200: You should run autoupdate.
./lib/autoconf/status.m4:719: AC_CONFIG_HEADER is expanded from...
configure.in:200: the top level
autoheader: warning: autoconf input should be named 'configure.ac', not 'configure.in'
# phpize 上面由于系统版本问题 可能比较麻烦 换条路。

## https://github.com/shivammathur/homebrew-extensions 这个太棒了！！！！

shaogaojie@shaogaojiedeMacBook-Pro memcache-3.0.8 % brew tap shivammathur/extensions
Running `brew update --auto-update`...
==> Auto-updated Homebrew!
Updated 2 taps (shivammathur/php and homebrew/services).

shaogaojie@shaogaojiedeMacBook-Pro memcache-3.0.8 %
shaogaojie@shaogaojiedeMacBook-Pro memcache-3.0.8 %
shaogaojie@shaogaojiedeMacBook-Pro memcache-3.0.8 %
shaogaojie@shaogaojiedeMacBook-Pro memcache-3.0.8 % brew install shivammathur/extensions/m
emcache@5.6
==> Downloading https://ghcr.io/v2/shivammathur/extensions/memcache/5.6/manifests/3.0.8
######################################################################## 100.0%
==> Downloading https://ghcr.io/v2/shivammathur/extensions/memcache/5.6/blobs/sha256:d7bd1
==> Downloading from https://pkg-containers.githubusercontent.com/ghcr1/blobs/sha256:d7bd1
######################################################################## 100.0%
==> Installing memcache@5.6 from shivammathur/extensions
==> Pouring memcache@5.6--3.0.8.big_sur.bottle.tar.gz
==> Caveats
To finish installing memcache for PHP 5.6:
  * /usr/local/etc/php/5.6/conf.d/memcache.ini was created,"
    do not forget to remove it upon extension removal."
  * Validate installation by running php -m
==> Summary
🍺  /usr/local/Cellar/memcache@5.6/3.0.8: 8 files, 144.3KB
==> Running `brew cleanup memcache@5.6`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).

# 安装成功啦。
/usr/local/Cellar/php@5.6/5.6.40_4/bin/php -m | grep memcache
memcache

# 重启服务
brew services restart shivammathur/php/php@5.6
```

##### redis.so

```shell
brew install shivammathur/extensions/redis@5.6
Running `brew update --auto-update`...
==> Auto-updated Homebrew!
Updated 1 tap (homebrew/cask).

==> Downloading https://ghcr.io/v2/homebrew/core/liblzf/manifests/3.6-1
######################################################################## 100.0%
==> Downloading https://ghcr.io/v2/homebrew/core/liblzf/blobs/sha256:c0a3c8f9311082eb797c848798f98da622
==> Downloading from https://pkg-containers.githubusercontent.com/ghcr1/blobs/sha256:c0a3c8f9311082eb79
######################################################################## 100.0%
==> Downloading https://ghcr.io/v2/shivammathur/extensions/igbinary/5.6/manifests/2.0.8-2
######################################################################## 100.0%
==> Downloading https://ghcr.io/v2/shivammathur/extensions/igbinary/5.6/blobs/sha256:0056fb6614d5048cc3
==> Downloading from https://pkg-containers.githubusercontent.com/ghcr1/blobs/sha256:0056fb6614d5048cc3
######################################################################## 100.0%
==> Downloading https://ghcr.io/v2/shivammathur/extensions/redis/5.6/manifests/4.3.0
######################################################################## 100.0%
==> Downloading https://ghcr.io/v2/shivammathur/extensions/redis/5.6/blobs/sha256:ed073690308558ea38e14
==> Downloading from https://pkg-containers.githubusercontent.com/ghcr1/blobs/sha256:ed073690308558ea38
######################################################################## 100.0%
==> Installing redis@5.6 from shivammathur/extensions
==> Installing dependencies for shivammathur/extensions/redis@5.6: liblzf and shivammathur/extensions/igbinary@5.6
==> Installing shivammathur/extensions/redis@5.6 dependency: liblzf
==> Pouring liblzf--3.6.monterey.bottle.1.tar.gz
🍺  /usr/local/Cellar/liblzf/3.6: 8 files, 69.6KB
==> Installing shivammathur/extensions/redis@5.6 dependency: shivammathur/extensions/igbinary@5.6
==> Pouring igbinary@5.6--2.0.8.monterey.bottle.2.tar.gz
🍺  /usr/local/Cellar/igbinary@5.6/2.0.8: 14 files, 101.7KB
==> Installing shivammathur/extensions/redis@5.6
==> Pouring redis@5.6--4.3.0.monterey.bottle.tar.gz
==> Caveats
To finish installing redis for PHP 5.6:
  * /usr/local/etc/php/5.6/conf.d/redis.ini was created,"
    do not forget to remove it upon extension removal."
  * Validate installation by running php -m
==> Summary
🍺  /usr/local/Cellar/redis@5.6/4.3.0: 15 files, 582.9KB
==> Running `brew cleanup redis@5.6`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
==> Caveats
==> redis@5.6
To finish installing redis for PHP 5.6:
  * /usr/local/etc/php/5.6/conf.d/redis.ini was created,"
    do not forget to remove it upon extension removal."
  * Validate installation by running php -m
shaogaojie@shaogaojiedeMacBook-Pro memcache-3.0.8 %
```



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

# 顺便将redis命令导入到环境变量
echo 'export PATH="/usr/local/opt/redis/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc


```



## 常用软件

### Sequel Pro

1. mysql 8.0 从该客户端登录的时候报错，解决方案：

   ```shell
   1. https://stackoverflow.com/questions/49194719/authentication-plugin-caching-sha2-password-cannot-be-loaded
   
   Authentication plugin 'caching_sha2_password' cannot be loaded: dlopen(/usr/local/mysql/lib/plugin/caching_sha2_password.so, 2): image not found
   
   # cli 登录Mysql 
   mysql --user=root --password
   # 执行下面命令 修改 password方式即可。
   ALTER USER 'yourusername'@'localhost' IDENTIFIED WITH mysql_native_password BY 'youpassword';
   
   ```

   

2. xx


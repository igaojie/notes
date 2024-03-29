# MYSQL 8.0 

## 安装

1. 下载

   ```shell
   # cd /data1/soft/
   #下载
   wget https://cdn.mysql.com/archives/mysql-8.0/mysql-8.0.26-linux-glibc2.12-x86_64.tar.xz
   --2021-11-19 15:10:52--  https://cdn.mysql.com/archives/mysql-8.0/mysql-8.0.26-linux-glibc2.12-x86_64.tar.xz
   正在解析主机 cdn.mysql.com (cdn.mysql.com)... 23.206.160.230
   正在连接 cdn.mysql.com (cdn.mysql.com)|23.206.160.230|:443... 已连接。
   已发出 HTTP 请求，正在等待回应... 200 OK
   长度：914806904 (872M) [text/plain]
   正在保存至: “mysql-8.0.26-linux-glibc2.12-x86_64.tar.xz”
   
   100%[============================================================================================================================>] 914,806,904 4.91MB/s 用时 3m 20s
   
   2021-11-19 15:14:13 (4.37 MB/s) - 已保存 “mysql-8.0.26-linux-glibc2.12-x86_64.tar.xz” [914806904/914806904])
   
   
   ```

   

2. 安装

   ```shell
   # 解压
   tar -xvf mysql-8.0.26-linux-glibc2.12-x86_64.tar.xz
   # 移到常用服务目录
   mv ./mysql-8.0.26-linux-glibc2.12-x86_64 /www/server/mysql-8.0.26
   
   # 由于当前机器已经安装了MySQL 5.6 版本，可以使用其 mysqld_multi 
   # 1. 自定义一个my.cnf 将数据目录新建好哦。不然会报错。
   [root@localhost mysql-8.0.26]# cat my.cnf
   [mysqld]
   socket     = /tmp/mysql.sock3308
   port       = 3308
   pid-file   = /data2/www/server/mysqldata/data3308/hostname.pid3308
   datadir    = /data2/www/server/mysqldata/data3308 #如果这个目录不存在 需要新建。
   
   
   
   #### 如果 datadir /data2/www/server/mysqldata/data3308  目录未创建，会报错：
   mysqld: Can't create directory '/data2/www/server/mysqldata/data3308/' (OS errno 2 - No such file or directory)
   2021-11-19T08:38:07.120299Z 0 [System] [MY-013169] [Server] /www/server/mysql-8.0.26/bin/mysqld (mysqld 8.0.26) initializing of server in progress as process 25555
   2021-11-19T08:38:07.124199Z 0 [ERROR] [MY-013236] [Server] The designated data directory /data2/www/server/mysqldata/data3308/ is unusable. You can remove all files that the server added to it.
   2021-11-19T08:38:07.124359Z 0 [ERROR] [MY-010119] [Server] Aborting
   2021-11-19T08:38:07.124642Z 0 [System] [MY-010910] [Server] /www/server/mysql-8.0.26/bin/mysqld: Shutdown complete (mysqld 8.0.26)  MySQL Community Server - GPL.
   
   
   # 2.初始化库
   ./bin/mysqld --defaults-file=/www/server/mysql-8.0.26/my.cnf --initialize --user=mysql
   
   2021-11-19T08:58:32.963548Z 0 [System] [MY-013169] [Server] /www/server/mysql-8.0.26/bin/mysqld (mysqld 8.0.26) initializing of server in progress as process 27552
   2021-11-19T08:58:32.978888Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
   
   
   
   2021-11-19T08:58:34.698028Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
   2021-11-19T08:58:36.233554Z 0 [Warning] [MY-013746] [Server] A deprecated TLS version TLSv1 is enabled for channel mysql_main
   2021-11-19T08:58:36.234350Z 0 [Warning] [MY-013746] [Server] A deprecated TLS version TLSv1.1 is enabled for channel mysql_main
   
   # 3. 看到这个就算初始化成功了 
   2021-11-19T08:58:36.666104Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: Ps%9W!Wyquo- # 记住 这个就是密码！！！！！！下边要用的啊。不然进不去了。
   
   # 4. /data2/www/server/mysqldata/data3308/ 目录下会生成基础文件 大功告成
   
   # 5. 由于本地机器上已经装过mysql 5.6 ,直接改他的配置文件 让mysqld_multi 能识别到即可。 
   # 追加以下文件到 /etc/my.cnf
   [mysqld_multi]
   mysqld     = /www/server/mysql/bin/mysqld_safe
   mysqladmin = /www/server/mysql/bin/mysqladmin
   user       = multi_admin
   password   = my_password  # mysqld_multi 需要使用 user 和 password 来控制 每个mysql服务的启停 所以需要配置并且在之前的5.6里面新增这个用户.
   
   
   [mysqld3308]
   server-id  = 3308
   mysqld     = /www/server/mysql-8.0.26/bin/mysqld_safe
   mysqladmin = /www/server/mysql-8.0.26/bin/mysqladmin
   socket     = /tmp/mysql.sock3308
   port       = 3308
   pid-file   = /data2/www/server/mysqldata/data3308/hostname.pid3308
   datadir    = /data2/www/server/mysqldata/data3308
   
   slow_query_log = 1
   slow_query_log_file = /data2/www/server/mysqldata/data3308/mysql-slow.log
   long_query_time = 2
   innodb_data_home_dir = /data2/www/server/mysqldata/data3308
   innodb_data_file_path = ibdata1:10M:autoextend
   innodb_log_group_home_dir = /data2/www/server/mysqldata/data3308
   default_authentication_plugin = mysql_native_password
   
   
   ```
## 启动

   

   ```shell
   # mysqld_multi report
   Reporting MySQL servers
   MySQL server from group: mysqld3308 is not running # 这个就是刚才新安装的3308端口的mysql 8.0 。
   # 可是我原来的mysql5.6  怎么没显示出来呢？
   # 那是因为 mysql5.6中没有 配置文件配合的mysqld_multi 所需要的user 和password匹配权限的账号。我们去增加一下。
   # 见下边的 6-1。
   
   #重新执行命令 你看 是不是都出来了。现在启动 mysqld3308。
   # mysqld_multi report
   Reporting MySQL servers
   MySQL server from group: mysqld3306 is running
   MySQL server from group: mysqld3308 is not running
   
   #启动3308
   mysqld_multi start 3308
   #检查是否有这个进程
   ps -ef | grep mysql
   root      1235     1  0 8月18 ?       00:00:00 /bin/sh /www/server/mysql/bin/mysqld_safe --datadir=/www/server/data --pid-file=/www/server/data/localhost.localdomain.pid --sql-mode=NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
   mysql     2705  1235  0 8月18 ?       02:56:23 /www/server/mysql/bin/mysqld --basedir=/www/server/mysql --datadir=/www/server/data --plugin-dir=/www/server/mysql/lib/plugin --user=mysql --sql-mode=NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION --log-error=localhost.localdomain.err --open-files-limit=65535 --pid-file=/www/server/data/localhost.localdomain.pid --socket=/tmp/mysql.sock --port=3306
   root     29835 25293  0 17:22 pts/2    00:00:00 mysql -uroot -p
   root     30558     1  0 17:30 pts/1    00:00:00 /bin/sh /www/server/mysql-8.0.26/bin/mysqld_safe --server-id=3308 --socket=/tmp/mysql.sock3308 --port=3308 --pid-file=/data2/www/server/mysqldata/data3308/hostname.pid3308 --datadir=/data2/www/server/mysqldata/data3308 --slow_query_log=1 --slow_query_log_file=/data2/www/server/mysqldata/data3308/mysql-slow.log --long_query_time=2 --innodb_data_home_dir=/data2/www/server/mysqldata/data3308 --innodb_data_file_path=ibdata1:10M:autoextend --innodb_log_group_home_dir=/data2/www/server/mysqldata/data3308 --default_authentication_plugin=mysql_native_password
   mysql    30844 30558 13 17:30 pts/1    00:00:01 /www/server/mysql-8.0.26/bin/mysqld --basedir=/www/server/mysql-8.0.26 --datadir=/data2/www/server/mysqldata/data3308 --plugin-dir=/www/server/mysql-8.0.26/lib/plugin --user=mysql --server-id=3308 --slow-query-log=1 --slow-query-log-file=/data2/www/server/mysqldata/data3308/mysql-slow.log --long-query-time=2 --innodb-data-home-dir=/data2/www/server/mysqldata/data3308 --innodb-data-file-path=ibdata1:10M:autoextend --innodb-log-group-home-dir=/data2/www/server/mysqldata/data3308 --default-authentication-plugin=mysql_native_password --log-error=localhost.localdomain.err --pid-file=/data2/www/server/mysqldata/data3308/hostname.pid3308 --socket=/tmp/mysql.sock3308 --port=3308
   root     30902 15222  0 17:30 pts/1    00:00:00 grep --color=auto mysql
   
   # lsof -i:3308
   COMMAND   PID  USER   FD   TYPE    DEVICE SIZE/OFF NODE NAME
   mysqld  30844 mysql   27u  IPv6 231572354      0t0  TCP *:tns-server (LISTEN)
   
   
   启动成功！！！
   
   ```

   

## 测试

   ```shell
   1. 进入mysql3308
   ./bin/mysql -h127.0.0.1 -P3308 -p
   Enter password: # 输入上面初始化数据库成功的时候 返回的密码。
   Welcome to the MySQL monitor.  Commands end with ; or \g.
   Your MySQL connection id is 10
   Server version: 8.0.26
   
   Copyright (c) 2000, 2021, Oracle and/or its affiliates.
   
   Oracle is a registered trademark of Oracle Corporation and/or its
   affiliates. Other names may be trademarks of their respective
   owners.
   
   Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
   
   mysql>
   mysql>
   mysql> select version();# 提示让修改密码。
   ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.
   mysql>
   mysql>
   mysql> alter user 'root'@'localhost' identified by 'youpassword'; # youpassword改为真实密码即可。
   Query OK, 0 rows affected (0.09 sec)
   
   mysql>
   mysql> select version();
   +-----------+
   | version() |
   +-----------+
   | 8.0.26    |
   +-----------+
   1 row in set (0.01 sec)
   
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
   
   mysql>
   
   # 好了。没问题。
   ```

4. 常见问题

   1.  5.6版本的mysql已经安装过了，现在想让mysqld_multi能控制和识别，需要在这个库中加入拥有启停权限的mysqld_multi 账号和密码。参考：https://dev.mysql.com/doc/refman/8.0/en/linux-installation-yum-repo.html

      ```mysql
      # mysql -uroot -p
      Enter password:
      Welcome to the MySQL monitor.  Commands end with ; or \g.
      Your MySQL connection id is 70
      Server version: 5.6.50-log Source distribution
      
      Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.
      
      Oracle is a registered trademark of Oracle Corporation and/or its
      affiliates. Other names may be trademarks of their respective
      owners.
      
      Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
      
      mysql>
      mysql>
      mysql> CREATE USER 'multi_admin'@'localhost' IDENTIFIED BY 'my_password';
      Query OK, 0 rows affected (0.00 sec)
      
      mysql> GRANT SHUTDOWN ON *.* TO 'multi_admin'@'localhost';
      Query OK, 0 rows affected (0.00 sec)
      ```

      

   2. mysqld_multi reload start stop不起作用的情况分析：

      1. mysqld_multi 账号和密码不正确。

      2. cn.cnf 配置文件(https://bugs.mysql.com/bug.php?id=77227)
   
         ```shell
         [mysqld_multi]
         mysqld     = /www/server/mysql/bin/mysqld_safe
         mysqladmin = /www/server/mysql/bin/mysqladmin
         user       = multi_admin
         password   = my_password # 看上面的那个页面 有人提到这个问题，改成下边的方式就好了。
         pass = my_password
         ```
   
         
   
   3. PHP5.6 Error: Unable to connect to MySQL. Debugging errno: 2054 Debugging error: Server sent charset unknown to the client. Please, report to the developers。https://stackoverflow.com/questions/43437490/pdo-construct-server-sent-charset-255-unknown-to-the-client-please-rep
   
      ```shell
      [client]
      default-character-set=utf8
      
      [mysql]
      default-character-set=utf8
      
      
      [mysqld]
      collation-server = utf8_unicode_ci
      character-set-server = utf8
      
      # 切记一定要重启mysql服务。
      ```
   
      
   
   4. 
   
   5. 22

# 配置项

## local-infile

## log-error

## symbolic-links



# Master-Slave 主从同步

## 现状

1. 174服务器上mysql3308 已经跑了N天，新建的mysql3308 是在192服务器。
2. 实现174服务器上mysql3308  数据 到 192服务器的mysql3308的数据同步，一主一备即可。
2. 实现主库不停的情况下，挂上从库。

## 开始

1. `全量备份`174（主）的数据。

   ```shell
   # 主库服务器执行全量备份操作
   cd /www/backup 
   
   xtrabackup --backup --host=127.0.0.1 --user=root --password='YOURPASSWORD' --port=3308 --target-dir=./x_backup
   xtrabackup: recognized server arguments:
   xtrabackup: recognized client arguments: --port=3306 --socket=/tmp/mysql.sock --backup=1 --host=127.0.0.1 --user=root --password=* --port=3308 --target-dir=./x_backup
   211119 20:34:56  version_check Connecting to MySQL server with DSN 'dbi:mysql:;mysql_read_default_group=xtrabackup;host=127.0.0.1;port=3308;mysql_socket=/tmp/mysql.sock' as 'root'  (using password: YES).
   211119 20:34:56  version_check Connected to MySQL server
   211119 20:34:56  version_check Executing a version check against the server...
   211119 20:34:56  version_check Done.
   211119 20:34:56 Connecting to MySQL server host: 127.0.0.1, user: root, password: set, port: 3308, socket: /tmp/mysql.sock
   ######啊啊啊啊啊啊啊啊啊啊  Xtrabackup 2.4.x 不支持。那就装 Xtrabackup 8.0吧。 
   Error: MySQL 8.0 and Percona Server 8.0 are not supported by Percona Xtrabackup 2.4.x series. Please use Percona Xtrabackup 8.0.x for backups and restores.
   
   # 先去安装 Xtrabackup 8.0 了。
   # 参考 Xtrabackup 章节。
   
   # 继续进行全量备份。
   # 第一步：：：
   (base) [root@localhost backup]# xtrabackup --backup --host=127.0.0.1 --user=root --password='1db81ea576e76d9e' --port=3308 --target-dir=./x_backup
   .................
   xtrabackup: recognized client arguments: --port=3306 --socket=/tmp/mysql.sock --backup=1 --host=127.0.0.1 --user=root --password=* --port=3308 --target-dir=./x_backup
   xtrabackup version 8.0.26-18 based on MySQL server 8.0.26 Linux (x86_64) (revision id: 4aecf82)
   211119 20:46:07  version_check Connecting to MySQL server with DSN 'dbi:mysql:;mysql_read_default_group=xtrabackup;host=127.0.0.1;port=3308;mysql_socket=/tmp/mysql.sock' as 'root'  (using password: YES).
   211119 20:46:07  version_check Connected to MySQL server
   211119 20:46:07  version_check Executing a version check against the server...
   211119 20:46:07  version_check Done.
   211119 20:46:07 Connecting to MySQL server host: 127.0.0.1, user: root, password: set, port: 3308, socket: /tmp/mysql.sock
   Using server version 8.0.26
   211119 20:46:07 Executing LOCK INSTANCE FOR BACKUP...
   xtrabackup: uses posix_fadvise().
   xtrabackup: cd to /www/server/mysqldata/data3308/
   xtrabackup: open files limit requested 0, set to 1024
   xtrabackup: using the following InnoDB configuration:
   xtrabackup:   innodb_data_home_dir = /www/server/mysqldata/data3308
   xtrabackup:   innodb_data_file_path = ibdata1:10M:autoextend
   xtrabackup:   innodb_log_group_home_dir = /www/server/mysqldata/data3308
   xtrabackup:   innodb_log_files_in_group = 2
   xtrabackup:   innodb_log_file_size = 50331648
   Number of pools: 1
   xtrabackup: inititialize_service_handles suceeded
   211119 20:46:07 Connecting to MySQL server host: 127.0.0.1, user: root, password: set, port: 3308, socket: /tmp/mysql.sock
   xtrabackup: Redo Log Archiving is not set up.
   211119 20:46:07 >> log scanned up to (26313712986)
   xtrabackup: Generating a list of tablespaces
   xtrabackup: Generating a list of tablespaces
   Scanning './'
   Completed space ID check of 2 files.
   211119 20:53:10 Executing UNLOCK INSTANCE
   211119 20:53:10 All tables unlocked
   211119 20:53:10 [00] Copying ib_buffer_pool to /www/backup/x_backup/ib_buffer_pool
   211119 20:53:10 [00]        ...done
   211119 20:53:10 Backup created in directory '/www/backup/x_backup/'
   MySQL binlog position: filename 'binlog.000003', position '156'
   211119 20:53:10 [00] Writing /www/backup/x_backup/backup-my.cnf
   211119 20:53:10 [00]        ...done
   211119 20:53:10 [00] Writing /www/backup/x_backup/xtrabackup_info
   211119 20:53:10 [00]        ...done
   xtrabackup: Transaction log of lsn (26313712986) to (26314972526) was copied.
   211119 20:53:11 completed OK!
   ...........
   
   # 第二步：：：
   # 执行 xtrabackup --prepare 
   
   # xtrabackup --prepare --host=127.0.0.1 --user=root --password='YOURPASSWORD' --port=3308 --target-dir=./x_backup
   xtrabackup: recognized server arguments: --innodb_checksum_algorithm=crc32 --innodb_log_checksums=1 --innodb_data_file_path=ibdata1:10M:autoextend --innodb_log_files_in_group=2 --innodb_log_file_size=50331648 --innodb_page_size=16384 --innodb_undo_directory=./ --innodb_undo_tablespaces=2 --server-id=0 --innodb_log_checksums=ON --innodb_redo_log_encrypt=0 --innodb_undo_log_encrypt=0
   xtrabackup: recognized client arguments: --prepare=1 --host=127.0.0.1 --user=root --password=* --port=3308 --target-dir=./x_backup
   xtrabackup version 8.0.26-18 based on MySQL server 8.0.26 Linux (x86_64) (revision id: 4aecf82)
   xtrabackup: cd to /www/backup/x_backup/
   xtrabackup: This target seems to be not prepared yet.
   Number of pools: 1
   xtrabackup: xtrabackup_logfile detected: size=8388608, start_lsn=(26313712986)
   xtrabackup: using the following InnoDB configuration for recovery:
   xtrabackup:   innodb_data_home_dir = .
   xtrabackup:   innodb_data_file_path = ibdata1:10M:autoextend
   xtrabackup:   innodb_log_group_home_dir = .
   xtrabackup:   innodb_log_files_in_group = 1
   xtrabackup:   innodb_log_file_size = 8388608
   xtrabackup: inititialize_service_handles suceeded
   xtrabackup: using the following InnoDB configuration for recovery:
   xtrabackup:   innodb_data_home_dir = .
   xtrabackup:   innodb_data_file_path = ibdata1:10M:autoextend
   xtrabackup:   innodb_log_group_home_dir = .
   xtrabackup:   innodb_log_files_in_group = 1
   xtrabackup:   innodb_log_file_size = 8388608
   xtrabackup: Starting InnoDB instance for recovery.
   xtrabackup: Using 104857600 bytes for buffer pool (set by --use-memory parameter)
   PUNCH HOLE support available
   Uses event mutexes
   GCC builtin __atomic_thread_fence() is used for memory barrier
   Compressed tables use zlib 1.2.11
   Number of pools: 1
   Using CPU crc32 instructions
   Directories to scan './'
   Scanning './'
   Completed space ID check of 69 files.
   Initializing buffer pool, total size = 128.000000M, instances = 1, chunk size =128.000000M
   Completed initialization of buffer pool
   page_cleaner coordinator priority: -20
   page_cleaner worker priority: -20
   page_cleaner worker priority: -20
   page_cleaner worker priority: -20
   The log sequence number 25189905561 in the system tablespace does not match the log sequence number 26313712986 in the ib_logfiles!
   Database was not shutdown normally!
   Starting crash recovery.
   Starting to parse redo log at lsn = 26313712685, whereas checkpoint_lsn = 26313712986 and start_lsn = 26313712640
   Doing recovery: scanned up to log sequence number 26314952544
   Log background threads are being started...
   Applying a batch of 991 redo log records ...
   10%
   20%
   30%
   40%
   50%
   60%
   70%
   80%
   90%
   100%
   Apply batch completed!
   Using undo tablespace './undo_001'.
   Using undo tablespace './undo_002'.
   Opened 2 existing undo tablespaces.
   GTID recovery trx_no: 8556730
   Parallel initialization of rseg complete
   Time taken to initialize rseg using 4 thread: 10331 ms.
   ........
   .....
   ....
   Starting shutdown...
   Log background threads are being closed...
   Shutdown completed; log sequence number 26314953238
   211119 20:54:52 completed OK!
   (base) [root@localhost backup]#
   .....................................................................
   
   ```

   

2. 同步174备份数据到192。

   ```shell
   # 在174服务器上执行 同步数据到 192。不打包了 直接传吧。。
   # rsync -avzP ./x_backup root@192.168.1.192:/data2/backup
   root@192.168.1.192's password: 
   sending incremental file list
   x_backup/
   x_backup/backup-my.cnf
               475 100%    0.00kB/s    0:00:00 (xfr#1, to-chk=204/206)
   x_backup/binlog.000003
               156 100%  152.34kB/s    0:00:00 (xfr#2, to-chk=203/206)
   x_backup/binlog.index
                16 100%   15.62kB/s    0:00:00 (xfr#3, to-chk=202/206)
   x_backup/ib_buffer_pool
           143,784 100%   12.47MB/s    0:00:00 (xfr#4, to-chk=201/206)
   x_backup/ib_logfile0
        50,331,648 100%   65.04MB/s    0:00:00 (xfr#5, to-chk=200/206)
   x_backup/ib_logfile1
        50,331,648 100%   33.90MB/s    0:00:01 (xfr#6, to-chk=199/206)
   x_backup/ibdata1
   ........
   x_backup/sys/sys_config.ibd
           114,688 100%  163.98kB/s    0:00:00 (xfr#199, to-chk=0/206)
   
   sent 1,938,594,981 bytes  received 3,841 bytes  4,102,854.65 bytes/sec
   total size is 12,044,096,061  speedup is 6.21
   
   
   # 在192上查看
   pwd
   /data2/backup
   
   [root@localhost backup]# ll
   drwxr-xr-x 3 root root 4096 11月 19 19:33 backup
   drwxr-x--- 8 root root 4096 11月 19 20:54 x_backup 
   
   
   # 同步成功 继续下一步。
   ```

   

3. 在192上将接收到的备份数据`全量恢复`

   192上没有安装percona xtrabackup ，先安装。按照步骤：https://www.percona.com/doc/percona-xtrabackup/2.4/installation/yum_repo.html

   

   ```shell
   # 1. 先停掉192上mysql 3308 必须停掉。
   
   mysqld_multi stop 3308  # 如果停止不了 ，检查 3308库上是否存在 mysqld_multi的那个账号和密码 。
   mysqld_multi report 
   [root@localhost backup]# mysqld_multi report
   Reporting MySQL servers
   MySQL server from group: mysqld3306 is running
   MySQL server from group: mysqld3308 is not running
   
   
   # 2.将3308库的datadir目录 进行备份重命名,保证3308库 datadir目录为空。
   cd /data2/www/server/mysqldata/
   mv data3308 data3308_old
   
   # 3. 全量恢复操作
   cd /data2/backup/  # 回到备份数据目录执行全量恢复。
   xtrabackup  --defaults-file=/www/server/mysql-8.0.26/my.cnf  --target-dir=./x_backup --copy-back  
   
   ###### --copy-back 保留数据进行恢复。不想保存备份，可以使用 -–move-back。
   211119 20:20:42 [01] Copying ./mysql/help_category.frm to /data2/www/server/mysqldata/data3308/mysql/help_category.frm
   211119 20:20:42 [01]        ...done
   211119 20:20:42 [01] Copying ./ibtmp1 to /data2/www/server/mysqldata/data3308/ibtmp1
   211119 20:20:43 [01]        ...done
   211119 20:20:43 completed OK!
   
   # 4.修改目录权限
   cd /data2/www/server/mysqldata/
   [root@localhost mysqldata]# ll
   总用量 8
   drwxr-x--- 8 root  root  4096 11月 19 20:20 data3308
   drwxr-xr-x 6 mysql mysql 4096 11月 19 20:14 data3308_old
   [root@localhost mysqldata]# chown -R mysql:mysql ./data3308
   
   
   # 恢复完成
   ```

   

4.  启动 从库

   ```shell
   # 启动
   mysqld_multi start 3308  --log=./mysqld_multi_tmp.log #加个日志 万一起不来 可以看看啥情况。
   
   mysqld_multi report
   Reporting MySQL servers
   MySQL server from group: mysqld3306 is running
   MySQL server from group: mysqld3308 is running
   
   # 好 ，起来了。@@
   
   ```

   

5. 开始同步

   ```shell
   # 1. 查看同步过来的数据 到了什么位置。binlog_pos 
   cat /data2/www/server/mysqldata/data3308/xtrabackup_info
   uuid = a6495e1e-4937-11ec-a1fb-b499baacf410
   name =
   tool_name = xtrabackup
   tool_command = --backup --host=127.0.0.1 --user=root --password=... --port=3308 --target-dir=./x_backup
   tool_version = 8.0.26-18
   ibbackup_version = 8.0.26-18
   server_version = 8.0.26
   start_time = 2021-11-19 20:46:07
   end_time = 2021-11-19 20:53:10
   lock_time = 2
   binlog_pos = filename 'binlog.000003', position '156'
   innodb_from_lsn = 0
   innodb_to_lsn = 26314745406
   partial = N
   incremental = N
   format = file
   compressed = N
   encrypted = N
   [root@localhost mysqldata]#
   
   #  binlog_pos = filename 'binlog.000003', position '156' 说明 需要从 filename 'binlog.000003', position '156' 开始同步。
   
   # 2. 在主库上新建一个用户 专门用来进行数据同步。
   mysql>
   mysql> grant  replication  slave  on *.* to  'slave3308'@'192.168.1.192'  identified by  'slave3308';
   ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'identified by  'slave3308'' at line 1
   # 出现这样的错误 ，是因为mysql 8.0 取消了这种创建用户并且赋权限的快捷方式操作。 需要分开来。
   # 参考：https://ma.ttias.be/mysql-8-removes-shorthand-creating-user-permissions/
   mysql>
   mysql>
   
   mysql> CREATE USER 'slave3308'@'192.168.1.192' IDENTIFIED BY 'slave3308';
   Query OK, 0 rows affected (0.13 sec)
   
   mysql> GRANT REPLICATION SLAVE ON *.* TO 'slave3308'@'192.168.1.192';
   Query OK, 0 rows affected (0.01 sec)
   
   
   # 检查是否创建成功
   mysql> select User from mysql.user\G;
   *************************** 1. row ***************************
   User: slave3308
   ......
   
   
   # 3. 去192从库做同步配置。
    mysql -h127.0.0.1 -P3308 -uroot -p # 从库服务器 192 ，登录3308
   Enter password:
   Welcome to the MySQL monitor.  Commands end with ; or \g.
   Your MySQL connection id is 9
   Server version: 8.0.26 MySQL Community Server - GPL
   
   Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.
   
   Oracle is a registered trademark of Oracle Corporation and/or its
   affiliates. Other names may be trademarks of their respective
   owners.
   
   Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
   
   mysql>
   mysql> CHANGE MASTER TO MASTER_HOST='192.168.1.174',
       ->     MASTER_PORT=3308,
       ->     MASTER_USER='slave3308',
       ->     MASTER_PASSWORD='slave3308',
       ->     MASTER_LOG_FILE='binlog.000003',
       ->     MASTER_LOG_POS=156;
   Query OK, 0 rows affected, 9 warnings (0.39 sec)
   
   
   # 开启 slave 。开始同步。
   mysql> start slave;
   Query OK, 0 rows affected, 1 warning (0.04 sec)
   
   
   mysql> show slave status\G;
   *************************** 1. row ***************************
                  Slave_IO_State: Waiting for source to send event
                     Master_Host: 192.168.1.174 # 主库host
                     Master_User: slave3308 #主库用户的用户名
                     Master_Port: 3308 #主库端口号
                   Connect_Retry: 60 
                 Master_Log_File: binlog.000003 # io线程拉取主库binlog的位置
             Read_Master_Log_Pos: 27444900 # io线程拉取主库binlog的位置
                  Relay_Log_File: localhost-relay-bin.000004 #sql线程执行relay log的位置
                   Relay_Log_Pos: 5576196 # sql线程执行relay log的位置
           Relay_Master_Log_File: binlog.000003 # sql线程执行的relay log相对于主库binlog的位置
                Slave_IO_Running: Yes # YES 
               Slave_SQL_Running: Yes # YES
                 Replicate_Do_DB:
             Replicate_Ignore_DB:
              Replicate_Do_Table:
          Replicate_Ignore_Table:
         Replicate_Wild_Do_Table:
     Replicate_Wild_Ignore_Table:
                      Last_Errno: 0
                      Last_Error:
                    Skip_Counter: 0
             Exec_Master_Log_Pos: 5576031 # sql线程执行的relay log相对于主库binlog的位置
                 Relay_Log_Space: 27445278 # 
                 Until_Condition: None
                  Until_Log_File:
                   Until_Log_Pos: 0
              Master_SSL_Allowed: No
              Master_SSL_CA_File:
              Master_SSL_CA_Path:
                 Master_SSL_Cert:
               Master_SSL_Cipher:
                  Master_SSL_Key:
           Seconds_Behind_Master: 3697  # 衡量master与slave之间延时的一个重要参数
   Master_SSL_Verify_Server_Cert: No
                   Last_IO_Errno: 0
                   Last_IO_Error:
                  Last_SQL_Errno: 0
                  Last_SQL_Error:
     Replicate_Ignore_Server_Ids:
                Master_Server_Id: 3308 # 主库 server-id
                     Master_UUID: 5c186035-20dd-11ec-a80a-b499baacf410
                Master_Info_File: mysql.slave_master_info
                       SQL_Delay: 0
             SQL_Remaining_Delay: NULL
         Slave_SQL_Running_State: waiting for handler commit
              Master_Retry_Count: 86400
                     Master_Bind:
         Last_IO_Error_Timestamp:
        Last_SQL_Error_Timestamp:
                  Master_SSL_Crl:
              Master_SSL_Crlpath:
              Retrieved_Gtid_Set:
               Executed_Gtid_Set:
                   Auto_Position: 0
            Replicate_Rewrite_DB:
                    Channel_Name:
              Master_TLS_Version:
          Master_public_key_path:
           Get_master_public_key: 0
               Network_Namespace:
   1 row in set, 1 warning (0.01 sec)
   
   
   
   mysql> show  master status\G;
   *************************** 1. row ***************************
                File: binlog.000009
            Position: 98124023
        Binlog_Do_DB:
    Binlog_Ignore_DB:
   Executed_Gtid_Set:
   1 row in set (0.03 sec)
   ```

   

6. 至此，我们也完成了主库不停机加从库的操作。Oh Ye！

## READ ONLY 

[`super_read_only`](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_super_read_only)

[`read_only`](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_read_only)

```mysql
# 设置只读
mysql> flush tables with read lock;
mysql> set global read_only=1;

#取消只读
mysql> unlock tables;
mysql> set global read_only=0;

# read_only=1只读模式，限定的是普通用户进行数据修改的操作，但不会限定具有super权限的用户的数据修改操作(但是如果设置了"super_read_only=on"， 则就会限定具有super权限的用户的数据修改操作了)；
```



## 问题

1. 延迟过高

​	 在从库执行 show slave status\G; 发现  Seconds_Behind_Master: 1812 这个一直在增大，特别是主库有批量插入的情况下。

​     通过查询官方文档，https://dev.mysql.com/doc/refman/8.0/en/replication-options-replica.html  ，mysql 8.0 提供了 [`replica_parallel_workers`](https://dev.mysql.com/doc/refman/8.0/en/replication-options-replica.html#sysvar_replica_parallel_workers) 的参数来并行复制。

```mysql
mysql> show variables like '%parallel_workers';
+--------------------------+-------+
| Variable_name            | Value |
+--------------------------+-------+
| replica_parallel_workers | 0     | # 默认为0 。从 MySQL 8.0.27开始，默认为4.
| slave_parallel_workers   | 0     | # 从MySQL 8.0.26开始，该参数会慢慢会替换掉，使用 replica_parallel_workers。默认为0
+--------------------------+-------+
2 rows in set (0.01 sec)

```

 登录从库执行： 

```shell
> set global replica_parallel_workers = 4;  
> stop slave;
> start slave;

# 执行以上操作后，发现依然同步很慢，并且 Seconds_Behind_Master 还是持续增长。必须用大招了

> show variables like 'sync_binlog';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| sync_binlog   | 1     |
+---------------+-------+
1 row in set (0.01 sec)

mysql> show variables like 'innodb_flush_log_at_trx_commit';
+--------------------------------+-------+
| Variable_name                  | Value |
+--------------------------------+-------+
| innodb_flush_log_at_trx_commit | 1     |
+--------------------------------+-------+
1 row in set (0.00 sec)

mysql> set global sync_binlog  = 0;
mysql > set global innodb_flush_log_at_trx_commit = 0;

# 眼看着 Seconds_Behind_Master 的数字在减小 。接下来研究下这两个参数的关系
```

2. innodb_flush_log_at_trx_commit和sync_binlog

   https://support.huaweicloud.com/bestpractice-rds/rds_02_0010.html

   **“innodb_flush_log_at_trx_commit”**和**“sync_binlog”**两个参数是控制MySQL磁盘写入策略以及数据安全性的关键参数。当两个参数为不同值时，在性能，安全角度下会产生不同的影响。

   **innodb_flush_log_at_trx_commit**：

   - 0：日志缓存区将每隔一秒写到日志文件中，并且将日志文件的数据刷新到磁盘上。该模式下在事务提交时不会主动触发写入磁盘的操作。当设置为0，该模式速度最快，但不太安全，mysqld进程的崩溃会导致上一秒钟所有事务数据的丢失；
   - 1：每次事务提交时MySQL都会把日志缓存区的数据写入日志文件中，并且刷新到磁盘中，该模式为系统默认。该模式是最安全的，但也是最慢的一种方式。在mysqld服务崩溃或者服务器主机宕机的情况下，日志缓存区只有可能丢失最多一个语句或者一个事务；
   - 2：每次事务提交时MySQL都会把日志缓存区的数据写入日志文件中，但是并不会同时刷新到磁盘上。该模式下，MySQL会每秒执行一次刷新磁盘操作。该模式速度较快，较取值为0情况下更安全，只有在操作系统崩溃或者系统断电的情况下，上一秒钟所有事务数据才可能丢失；

   **sync_binlog=1 or N：**

   - 默认情况下，并不是每次写入时都将binlog日志文件与磁盘同步。因此如果操作系统或服务器崩溃，有可能binlog中最后的语句丢失。

   - 为了防止这种情况，你可以使用**“sync_binlog”**全局变量（1是最安全的值，但也是最慢的），使binlog在每N次binlog日志文件写入后与磁盘同步。

![image-20211123145125781](/Users/shaogaojie/Library/Application Support/typora-user-images/image-20211123145125781.png)





2. 跳过错误 继续同步

```mysql
STOP SLAVE;
Query OK, 0 rows affected, 1 warning (0.07 sec)

mysql> SET GLOBAL SQL_SLAVE_SKIP_COUNTER = 1;
Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql> start slave;
Query OK, 0 rows affected, 1 warning (0.05 sec)
```

3.  试试

4. 哈哈



# Xtrabackup

## 安装

https://www.percona.com/doc/percona-xtrabackup/2.4/index.html

https://www.percona.com/doc/percona-xtrabackup/2.4/installation/yum_repo.html

1. 安装**percona-release** 配置工具

```
yum install https://repo.percona.com/yum/percona-release-latest.noarch.rpm

For more information, please visit:
  https://www.percona.com/doc/percona-repo-config/percona-release.html

  验证中      : percona-release-1.0-27.noarch                                                       1/1

已安装:
  percona-release.noarch 0:1.0-27

完毕！


```

2. 检查仓库

   ```shell
   yum list | grep percona
   percona-release.noarch                     1.0-27                 @/percona-release-latest.noarch
   Percona-Server-55-debuginfo.x86_64         5.5.62-rel38.14.el7    percona-release-x86_64
   Percona-Server-56-debuginfo.x86_64         5.6.51-rel91.0.1.el7   percona-release-x86_64
   Percona-Server-57-debuginfo.x86_64         5.7.35-38.1.el7        percona-release-x86_64
   Percona-Server-80-info.x86_64              8.0-1.el7              percona-release-x86_64
   Percona-Server-MongoDB.x86_64              3.0.15-1.10.el7        percona-release-x86_64
   Percona-Server-MongoDB-32.x86_64           3.2.22-3.13.el7        percona-release-x86_64
   Percona-Server-MongoDB-32-debuginfo.x86_64 3.2.22-3.13.el7        percona-release-x86_64
   Percona-Server-MongoDB-32-mongos.x86_64    3.2.22-3.13.el7        percona-release-x86_64
   Percona-Server-MongoDB-32-server.x86_64    3.2.22-3.13.el7        percona-release-x86_64
   Percona-Server-MongoDB-32-shell.x86_64     3.2.22-3.13.el7        percona-release-x86_64
   Percona-Server-MongoDB-32-tools.x86_64     3.2.22-3.13.el7        percona-release-x86_64
   Percona-Server-MongoDB-34.x86_64           3.4.24-3.0.el7         percona-release-x86_64
   Percona-Server-MongoDB-34-debuginfo.x86_64 3.4.24-3.0.el7         percona-release-x86_64
   Percona-Server-MongoDB-34-mongos.x86_64    3.4.24-3.0.el7         percona-release-x86_64
   Percona-Server-MongoDB-34-server.x86_64    3.4.24-3.0.el7         percona-release-x86_64
   Percona-Server-MongoDB-34-shell.x86_64     3.4.24-3.0.el7         percona-release-x86_64
   Percona-Server-MongoDB-34-tools.x86_64     3.4.24-3.0.el7         percona-release-x86_64
   Percona-Server-MongoDB-36.x86_64           3.6.23-13.0.el7        percona-release-x86_64
   Percona-Server-MongoDB-36-debuginfo.x86_64 3.6.23-13.0.el7        percona-release-x86_64
   Percona-Server-MongoDB-36-mongos.x86_64    3.6.23-13.0.el7        percona-release-x86_64
   Percona-Server-MongoDB-36-server.x86_64    3.6.23-13.0.el7        percona-release-x86_64
   Percona-Server-MongoDB-36-shell.x86_64     3.6.23-13.0.el7        percona-release-x86_64
   Percona-Server-MongoDB-36-tools.x86_64     3.6.23-13.0.el7        percona-release-x86_64
   Percona-Server-MongoDB-debuginfo.x86_64    3.0.15-1.10.el7        percona-release-x86_64
   Percona-Server-MongoDB-mongos.x86_64       3.0.15-1.10.el7        percona-release-x86_64
   Percona-Server-MongoDB-server.x86_64       3.0.15-1.10.el7        percona-release-x86_64
   Percona-Server-MongoDB-shell.x86_64        3.0.15-1.10.el7        percona-release-x86_64
   Percona-Server-MongoDB-tools.x86_64        3.0.15-1.10.el7        percona-release-x86_64
   Percona-Server-client-55.x86_64            5.5.62-rel38.14.el7    percona-release-x86_64
   Percona-Server-client-56.x86_64            5.6.51-rel91.0.1.el7   percona-release-x86_64
   Percona-Server-client-57.x86_64            5.7.35-38.1.el7        percona-release-x86_64
   Percona-Server-devel-55.x86_64             5.5.62-rel38.14.el7    percona-release-x86_64
   Percona-Server-devel-56.x86_64             5.6.51-rel91.0.1.el7   percona-release-x86_64
   Percona-Server-devel-57.x86_64             5.7.35-38.1.el7        percona-release-x86_64
   Percona-Server-rocksdb-57.x86_64           5.7.35-38.1.el7        percona-release-x86_64
   Percona-Server-selinux-55.noarch           5.5.62-rel38.14.el7    percona-release-x86_64
   Percona-Server-selinux-56.noarch           5.6.51-rel91.0.1.el7   percona-release-x86_64
   Percona-Server-server-55.x86_64            5.5.62-rel38.14.el7    percona-release-x86_64
   Percona-Server-server-56.x86_64            5.6.51-rel91.0.1.el7   percona-release-x86_64
   Percona-Server-server-57.x86_64            5.7.35-38.1.el7        percona-release-x86_64
   Percona-Server-shared-55.x86_64            5.5.62-rel38.14.el7    percona-release-x86_64
   Percona-Server-shared-56.x86_64            5.6.51-rel91.0.1.el7   percona-release-x86_64
   Percona-Server-shared-57.x86_64            5.7.35-38.1.el7        percona-release-x86_64
   Percona-Server-shared-compat-57.x86_64     5.7.35-38.1.el7        percona-release-x86_64
   Percona-Server-test-55.x86_64              5.5.62-rel38.14.el7    percona-release-x86_64
   Percona-Server-test-56.x86_64              5.6.51-rel91.0.1.el7   percona-release-x86_64
   Percona-Server-test-57.x86_64              5.7.35-38.1.el7        percona-release-x86_64
   Percona-Server-tokudb-56.x86_64            5.6.51-rel91.0.1.el7   percona-release-x86_64
   Percona-Server-tokudb-57.x86_64            5.7.35-38.1.el7        percona-release-x86_64
   Percona-XtraDB-Cluster-55.x86_64           1:5.5.41-25.12.855.el7 percona-release-x86_64
   Percona-XtraDB-Cluster-55-debuginfo.x86_64 1:5.5.41-25.12.855.el7 percona-release-x86_64
   Percona-XtraDB-Cluster-55-g3.x86_64        1:5.5.41-25.12.855.el7 percona-release-x86_64
   Percona-XtraDB-Cluster-56.x86_64           1:5.6.51-28.46.1.el7   percona-release-x86_64
   Percona-XtraDB-Cluster-56-debuginfo.x86_64 1:5.6.51-28.46.1.el7   percona-release-x86_64
   Percona-XtraDB-Cluster-57.x86_64           5.7.35-31.53.1.el7     percona-release-x86_64
   Percona-XtraDB-Cluster-57-debuginfo.x86_64 5.7.35-31.53.1.el7     percona-release-x86_64
   Percona-XtraDB-Cluster-client-55.x86_64    1:5.5.41-25.12.855.el7 percona-release-x86_64
   Percona-XtraDB-Cluster-client-56.x86_64    1:5.6.51-28.46.1.el7   percona-release-x86_64
   Percona-XtraDB-Cluster-client-57.x86_64    5.7.35-31.53.1.el7     percona-release-x86_64
   Percona-XtraDB-Cluster-devel-55.x86_64     1:5.5.41-25.12.855.el7 percona-release-x86_64
   Percona-XtraDB-Cluster-devel-56.x86_64     1:5.6.51-28.46.1.el7   percona-release-x86_64
   Percona-XtraDB-Cluster-devel-57.x86_64     5.7.35-31.53.1.el7     percona-release-x86_64
   Percona-XtraDB-Cluster-full-55.x86_64      1:5.5.41-25.12.855.el7 percona-release-x86_64
   Percona-XtraDB-Cluster-full-55-g3.x86_64   1:5.5.41-25.12.855.el7 percona-release-x86_64
   Percona-XtraDB-Cluster-full-56.x86_64      1:5.6.51-28.46.1.el7   percona-release-x86_64
   Percona-XtraDB-Cluster-full-57.x86_64      5.7.35-31.53.1.el7     percona-release-x86_64
   Percona-XtraDB-Cluster-galera-2.x86_64     2.12-1.2682.rhel7      percona-release-x86_64
                                              2.12-1.2682.rhel7      percona-release-x86_64
   Percona-XtraDB-Cluster-galera-3.x86_64     3.46-1.el7             percona-release-x86_64
                                              3.46-1.el7             percona-release-x86_64
   Percona-XtraDB-Cluster-garbd-2.x86_64      2.12-1.2682.rhel7      percona-release-x86_64
   Percona-XtraDB-Cluster-garbd-3.x86_64      3.46-1.el7             percona-release-x86_64
   Percona-XtraDB-Cluster-garbd-57.x86_64     5.7.35-31.53.1.el7     percona-release-x86_64
   Percona-XtraDB-Cluster-server-55.x86_64    1:5.5.41-25.12.855.el7 percona-release-x86_64
   Percona-XtraDB-Cluster-server-56.x86_64    1:5.6.51-28.46.1.el7   percona-release-x86_64
   Percona-XtraDB-Cluster-server-57.x86_64    5.7.35-31.53.1.el7     percona-release-x86_64
   Percona-XtraDB-Cluster-shared-55.x86_64    1:5.5.41-25.12.855.el7 percona-release-x86_64
   Percona-XtraDB-Cluster-shared-56.x86_64    1:5.6.51-28.46.1.el7   percona-release-x86_64
   Percona-XtraDB-Cluster-shared-57.x86_64    5.7.35-31.53.1.el7     percona-release-x86_64
                                              5.7.35-31.53.1.el7     percona-release-x86_64
   Percona-XtraDB-Cluster-test-55.x86_64      1:5.5.41-25.12.855.el7 percona-release-x86_64
   Percona-XtraDB-Cluster-test-56.x86_64      1:5.6.51-28.46.1.el7   percona-release-x86_64
   Percona-XtraDB-Cluster-test-57.x86_64      5.7.35-31.53.1.el7     percona-release-x86_64
   jemalloc-debuginfo.x86_64                  3.3.1-1.el7            percona-release-x86_64
   libeatmydata.x86_64                        0.1-00.21.el7.centos   percona-release-x86_64
   libeatmydata-debuginfo.x86_64              0.1-00.21.el7.centos   percona-release-x86_64
   percona-cacti-templates.noarch             1.1.8-1                percona-release-noarch
   percona-nagios-plugins.noarch              1.1.8-1                percona-release-noarch
   percona-toolkit.noarch                     2.2.20-1               percona-release-noarch
   percona-toolkit.x86_64                     3.3.1-1.el7            percona-release-x86_64
   percona-toolkit-debuginfo.x86_64           3.0.13-1.el7           percona-release-x86_64
   percona-xtrabackup.x86_64                  2.3.10-1.el7           percona-release-x86_64
   percona-xtrabackup-22.x86_64               2.2.13-1.el7           percona-release-x86_64
   percona-xtrabackup-22-debuginfo.x86_64     2.2.13-1.el7           percona-release-x86_64
   percona-xtrabackup-24.x86_64               2.4.24-1.el7           percona-release-x86_64
   percona-xtrabackup-24-debuginfo.x86_64     2.4.24-1.el7           percona-release-x86_64
   percona-xtrabackup-80.x86_64               8.0.26-18.1.el7        percona-release-x86_64
   percona-xtrabackup-80-debuginfo.x86_64     8.0.26-18.1.el7        percona-release-x86_64
   percona-xtrabackup-debuginfo.x86_64        2.3.10-1.el7           percona-release-x86_64
   percona-xtrabackup-test.x86_64             2.3.10-1.el7           percona-release-x86_64
   percona-xtrabackup-test-22.x86_64          2.2.13-1.el7           percona-release-x86_64
   percona-xtrabackup-test-24.x86_64          2.4.24-1.el7           percona-release-x86_64
   percona-xtrabackup-test-80.x86_64          8.0.26-18.1.el7        percona-release-x86_64
   percona-zabbix-templates.noarch            1.1.8-1                percona-release-noarch
   pmm-client.x86_64                          1.17.4-1.el7           percona-release-x86_64
   pmm2-client.x86_64                         2.24.0-6.el7           percona-release-x86_64
   proxysql.x86_64                            1.4.16-1.1.el7         percona-release-x86_64
   proxysql2.x86_64                           2.3.2-1.1.el7          percona-release-x86_64
   qpress.x86_64                              11-1.el7               percona-release-x86_64
   qpress-debuginfo.x86_64                    11-1.el7               percona-release-x86_64
   sysbench.x86_64                            1.0.20-6.el7           percona-release-x86_64
   sysbench-debuginfo.x86_64                  1.0.20-6.el7           percona-release-x86_64
   sysbench-tpcc.x86_64                       1.0.20-6.el7           percona-release-x86_64
   (base) [root@localhost data]#
   ```

   

3. 开启

   ```shell
   percona-release enable-only tools release
   * Disabling all Percona Repositories
   * Enabling the Percona Tools repository
   <*> All done!
   
   ```

   

4. 安装 Xtrabackup工具

   ```shell
   yum install percona-xtrabackup-24
   ```

5. 安装成功

   ```shell
   # xtrabackup -v
   xtrabackup: recognized server arguments:
   xtrabackup version 2.4.24 based on MySQL server 5.7.35 Linux (x86_64) (revision id: b4ee263)
   
   ```
   
5. 卸载 2.4版本

   ```shell
   yum remove percona-xtrabackup-24
   ```
   
   
   
5. 安装 

   ```shell
   yum install  percona-xtrabackup-80
   ```
   
   
   
8. 如何让两个版本共存？percona-xtrabackup-80能否处理 低版本的mysql的备份恢复呢？疑问？



## 安装异常情况

```shell
https://www.percona.com/doc/percona-xtrabackup/2.4/installation/yum_repo.html

yum install https://repo.percona.com/yum/percona-release-latest.noarch.rpm
上次元数据过期检查：1:11:40 前，执行于 2022年04月03日 星期日 08时19分03秒。
percona-release-latest.noarch.rpm                         17 kB/s |  20 kB     00:01
依赖关系解决。
=========================================================================================
 软件包                   架构            版本               仓库                   大小
=========================================================================================
安装:
 percona-release          noarch          1.0-27             @commandline           20 k

事务概要
=========================================================================================
安装  1 软件包

总计：20 k
安装大小：32 k
确定吗？[y/N]： y
下载软件包：
运行事务检查
事务检查成功。
运行事务测试
事务测试成功。
运行事务
  准备中  :                                                                          1/1
  安装    : percona-release-1.0-27.noarch                                            1/1
  运行脚本: percona-release-1.0-27.noarch                                            1/1
错误：无法创建 事务 锁定于 /var/lib/rpm/.rpm.lock (资源暂时不可用)
错误：/etc/pki/rpm-gpg/RPM-GPG-KEY-Percona：导入密钥 1 失败。
Specified repository is not supported for current operating system!
Specified repository is not supported for current operating system!
The percona-release package now contains a percona-release script that can enable additional repositories for our newer products.

For example, to enable the Percona Server 8.0 repository use:

  percona-release setup ps80

Note: To avoid conflicts with older product versions, the percona-release setup command may disable our original repository for some products.

For more information, please visit:
  https://www.percona.com/doc/percona-repo-config/percona-release.html


  验证    : percona-release-1.0-27.noarch                                            1/1

已安装:
  percona-release-1.0-27.noarch

完毕！
[root@xinqiao ~]# yum list | grep percona
percona-release.noarch                                            1.0-27                                          @@commandline
[root@xinqiao ~]# percona-release enable-only tools release
Specified repository is not supported for current operating system!  $$$$$竟然提示不支持！！！！！！！ 
[root@xinqiao ~]#
[root@xinqiao ~]#

那只能源码安装了


# 下载rpm包
 wget https://www.percona.com/downloads/XtraBackup/Percona-XtraBackup-2.4.4/\
> binary/redhat/7/x86_64/percona-xtrabackup-24-2.4.4-1.el7.x86_64.rpm
--2022-04-03 09:34:05--  https://www.percona.com/downloads/XtraBackup/Percona-XtraBackup-2.4.4/binary/redhat/7/x86_64/percona-xtrabackup-24-2.4.4-1.el7.x86_64.rpm
正在解析主机 www.percona.com (www.percona.com)... 172.67.8.157, 104.22.8.28, 104.22.9.28, ...
正在连接 www.percona.com (www.percona.com)|172.67.8.157|:443... 已连接。
已发出 HTTP 请求，正在等待回应... 301 Moved Permanently
位置：https://downloads.percona.com/downloads/XtraBackup/Percona-XtraBackup-2.4.4/binary/redhat/7/x86_64/percona-xtrabackup-24-2.4.4-1.el7.x86_64.rpm [跟随至新的 URL]
--2022-04-03 09:34:07--  https://downloads.percona.com/downloads/XtraBackup/Percona-XtraBackup-2.4.4/binary/redhat/7/x86_64/percona-xtrabackup-24-2.4.4-1.el7.x86_64.rpm
正在解析主机 downloads.percona.com (downloads.percona.com)... 162.220.4.222, 162.220.4.221, 74.121.199.231
正在连接 downloads.percona.com (downloads.percona.com)|162.220.4.222|:443... 已连接。
已发出 HTTP 请求，正在等待回应... 200 OK
长度：7839980 (7.5M) [application/octet-stream]
正在保存至: “percona-xtrabackup-24-2.4.4-1.el7.x86_64.rpm”

percona-xtrabackup-24- 100%[=========================>]   7.48M  3.13MB/s  用时 2.4s

2022-04-03 09:34:10 (3.13 MB/s) - 已保存 “percona-xtrabackup-24-2.4.4-1.el7.x86_64.rpm” [7839980/7839980])

# 也不行啊 要求libgcrypt的版本。 
yum localinstall percona-xtrabackup-24-2.4.4-1.el7.x86_64.rpm
上次元数据过期检查：1:17:09 前，执行于 2022年04月03日 星期日 08时19分03秒。
错误：
 问题: conflicting requests
  - nothing provides libgcrypt.so.11()(64bit) needed by percona-xtrabackup-24-2.4.4-1.el7.x86_64
  - nothing provides libgcrypt.so.11(GCRYPT_1.2)(64bit) needed by percona-xtrabackup-24-2.4.4-1.el7.x86_64
(尝试添加 '--skip-broken' 来跳过无法安装的软件包 或 '--nobest' 来不只使用软件包的最佳候选)


# 试着从官网找找能不能找到？
https://www.percona.com/downloads/Percona-XtraBackup-2.4/Percona-XtraBackup-2.4.14/binary/tarball/

# 下个试试
wget https://downloads.percona.com/downloads/Percona-XtraBackup-2.4/Percona-XtraBackup-2.4.18/binary/tarball/percona-xtrabackup-2.4.18-Linux-x86_64.libgcrypt183.tar.gz
tar xf percona-xtrabackup-2.4.18-Linux-x86_64.libgcrypt183.tar.gz
 cd percona-xtrabackup-2.4.18-Linux-x86_64/
 tar xf percona-xtrabackup-2.4.18-Linux-x86_64.libgcrypt183.tar.gz
 mv percona-xtrabackup-2.4.18-Linux-x86_64 /usr/local/xtrabackup
 echo "export PATH=$PATH:/usr/local/xtrabackup/bin" >> /etc/profile
 source /etc/profile
 xtrabackup --version
 
 
 xtrabackup --version
xtrabackup: recognized server arguments: --datadir=/data/www/server/data3306 --open_files_limit=65535 --log_bin=mysql-bin --server-id=1 --innodb_data_home_dir=/data/www/server/data3306 --innodb_data_file_path=ibdata1:10M:autoextend --innodb_log_group_home_dir=/data/www/server/data3306 --innodb_buffer_pool_size=256M --innodb_log_file_size=128M --innodb_log_buffer_size=32M --innodb_flush_log_at_trx_commit=1 --innodb_max_dirty_pages_pct=90 --innodb_read_io_threads=2 --innodb_write_io_threads=2
xtrabackup version 2.4.18 based on MySQL server 5.7.26 Linux (x86_64) (revision id: 29b4ca5)

```



## 备份

### 检查主库

```mysql
mysql> show variables like "log_bin";
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| log_bin       | ON    |
+---------------+-------+
1 row in set (0.00 sec)

mysql> show variables like "%log_bin%";
+---------------------------------+---------------------------------------------+
| Variable_name                   | Value                                       |
+---------------------------------+---------------------------------------------+
| log_bin                         | ON                                          |
| log_bin_basename                | /www/server/mysqldata/data3308/binlog       |
| log_bin_index                   | /www/server/mysqldata/data3308/binlog.index |
| log_bin_trust_function_creators | OFF                                         |
| log_bin_use_v1_row_events       | OFF                                         |
| sql_log_bin                     | ON                                          |
+---------------------------------+---------------------------------------------+
6 rows in set (0.01 sec)
```



# MYSQLD_MULTI

## 检查状态

```shell
# mysqld_multi report
Reporting MySQL servers
MySQL server from group: mysqld3306 is running
MySQL server from group: mysqld3308 is running
```



# 	SQL

## SQL_MODEL

1. sql_mode=only_full_group_by

# ALTER

## 字符集

1. 修改数据表与表中字段的编码及字符集

```mysql

SELECT 
CONCAT("ALTER TABLE `", TABLE_NAME,"` CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;") 
AS target_tables
FROM INFORMATION_SCHEMA.TABLES
WHERE TABLE_SCHEMA="数据库名字"
AND TABLE_TYPE="BASE TABLE";

上面语句会生成库里所有的表需要修改编码和字符集的SQL语句。
```



## ADD

```mysql
# 给已经存在的表并且非空 增加一个自增ID
ALTER TABLE tbl_name ADD id INT PRIMARY KEY AUTO_INCREMENT;
```

# 全文索引

## 全文解析器**Ngram**

1. 可以支持中文、日文、韩文分词

2. 只有char、varchar、text类型字段能创建全文索引

3. 英文分词用空格，逗号；中文分词用 ngram_token_size 设定。

4. 模式

   1. **自然语言模式(NATURAL LANGUAGE MODE)**

   2. **BOOLEAN模式(BOOLEAN MODE)**

      BOOLEAN模式可以使用操作符，可以支持指定关键词必须出现或者必须不能出现或者关键词的权重高还是低等复杂查询

      ```
      1.'dog cat'
      字段当中加一个空格，表示查询结果中包含dog或者cat
      2.'+dog +cat'
      字段前面加'+'，表示必须同时包含dog和cat
      3.'+dog cat'
      表示必须包含dog，对cat并不强制，如果出现cat，则相关性会更高
      4.'+dog -cat'
      表示必须包含dog，必须不包含cat
      5.'+dog ~cat'
      表示必须包含dog，对cat并不强制，如果出现cat，则相关性会变低
      6.'+dog +(>cat <pig)'
      表示必须包含dog和cat或者包含dog和pig,但是dog cat的相关性比dog pig的相关性高
      7.'dog*'
      表示包含以dog开头的内容
      
      ```

```mysql
/* 14:31:30 bingli[生产] bingli */ SELECT * FROM all_repeat_reports_ngram 
WHERE MATCH (YAOFANG) AGAINST ('+枸杞 -阿胶' in boolean mode) 
and MATCH(XIANGBINGSHI) AGAINST('纳眠' IN BOOLEAN MODE)
;

/* 14:24:45 bingli[生产] bingli */ ALTER TABLE all_repeat_reports_ngram ADD FULLTEXT INDEX idx_yaofang ( YAOFANG ) WITH PARSER ngram;

/* 14:21:15 bingli[生产] bingli */ ALTER TABLE `all_repeat_reports_ngram` ADD FULLTEXT INDEX `idx_zhusu` (`ZHUSU`) WITH PARSER ngram;

/* 14:21:15 bingli[生产] bingli */ ALTER TABLE `all_repeat_reports_ngram` ADD FULLTEXT INDEX `idx_xianbingshi` (`XIANGBINGSHI`) WITH PARSER ngram;


```



# pt-online-schema-change

1. 使用场景

   在线数据库的维护中，总会涉及到研发修改表结构的情况，修改一些小表影响很小，而修改大表时，往往影响业务的正常运转，如表数据量超过500W，1000W，甚至过亿时

2. 可能的影响

   - 在线修改大表的表结构执行时间往往不可预估，一般时间较长
   - 由于修改表结构是表级锁，因此在修改表结构时，影响表写入操作
   - 如果长时间的修改表结构，中途修改失败，由于修改表结构是一个事务，因此失败后会还原表结构，在这个过程中表都是锁着不可写入
   - 修改大表结构容易导致数据库CPU、IO等性能消耗，使MySQL服务器性能降低
   - 在线修改大表结构容易导致主从延时，从而影响业务读取

3. 原理

   - 首先它会新建一张一模一样的表，表名一般是_new后缀
   - 然后在这个新表执行更改字段操作
   - 然后在原表上加三个触发器，DELETE/UPDATE/INSERT，将原表中要执行的语句也在新表中执行
   - 最后将原表的数据拷贝到新表中，然后替换掉原表

4. 操作

# mysqld_multi

```
[root@localhost ~]# mysqld_multi --example
# This is an example of a my.cnf file for mysqld_multi.
# Usually this file is located in home dir ~/.my.cnf or /etc/my.cnf
#
# SOME IMPORTANT NOTES FOLLOW:
#
# 1.COMMON USER
#
#   Make sure that the MySQL user, who is stopping the mysqld services, has
#   the same password to all MySQL servers being accessed by mysqld_multi.
#   This user needs to have the 'Shutdown_priv' -privilege, but for security
#   reasons should have no other privileges. It is advised that you create a
#   common 'multi_admin' user for all MySQL servers being controlled by
#   mysqld_multi. Here is an example how to do it:
#
#   GRANT SHUTDOWN ON *.* TO multi_admin@localhost IDENTIFIED BY 'password'
#
#   You will need to apply the above to all MySQL servers that are being
#   controlled by mysqld_multi. 'multi_admin' will shutdown the servers
#   using 'mysqladmin' -binary, when 'mysqld_multi stop' is being called.
#
# 2.PID-FILE
#
#   If you are using mysqld_safe to start mysqld, make sure that every
#   MySQL server has a separate pid-file. In order to use mysqld_safe
#   via mysqld_multi, you need to use two options:
#
#   mysqld=/path/to/mysqld_safe
#   ledir=/path/to/mysqld-binary/
#
#   ledir (library executable directory), is an option that only mysqld_safe
#   accepts, so you will get an error if you try to pass it to mysqld directly.
#   For this reason you might want to use the above options within [mysqld#]
#   group directly.
#
# 3.DATA DIRECTORY
#
#   It is NOT advised to run many MySQL servers within the same data directory.
#   You can do so, but please make sure to understand and deal with the
#   underlying caveats. In short they are:
#   - Speed penalty
#   - Risk of table/data corruption
#   - Data synchronising problems between the running servers
#   - Heavily media (disk) bound
#   - Relies on the system (external) file locking
#   - Is not applicable with all table types. (Such as InnoDB)
#     Trying so will end up with undesirable results.
#
# 4.TCP/IP Port
#
#   Every server requires one and it must be unique.
#
# 5.[mysqld#] Groups
#
#   In the example below the first and the fifth mysqld group was
#   intentionally left out. You may have 'gaps' in the config file. This
#   gives you more flexibility.
#
# 6.MySQL Server User
#
#   You can pass the user=... option inside [mysqld#] groups. This
#   can be very handy in some cases, but then you need to run mysqld_multi
#   as UNIX root.
#
# 7.A Start-up Manage Script for mysqld_multi
#
#   In the recent MySQL distributions you can find a file called
#   mysqld_multi.server.sh. It is a wrapper for mysqld_multi. This can
#   be used to start and stop multiple servers during boot and shutdown.
#
#   You can place the file in /etc/init.d/mysqld_multi.server.sh and
#   make the needed symbolic links to it from various run levels
#   (as per Linux/Unix standard). You may even replace the
#   /etc/init.d/mysql.server script with it.
#
#   Before using, you must create a my.cnf file either in /www/server/mysql/my.cnf
#   or /root/.my.cnf and add the [mysqld_multi] and [mysqld#] groups.
#
#   The script can be found from support-files/mysqld_multi.server.sh
#   in MySQL distribution. (Verify the script before using)
#

[mysqld_multi]
mysqld     = /www/server/mysql/bin/mysqld_safe
mysqladmin = /www/server/mysql/bin/mysqladmin
user       = multi_admin
password   = my_password

[mysqld2]
socket     = /tmp/mysql.sock2
port       = 3307
pid-file   = /www/server/mysql/data2/hostname.pid2
datadir    = /www/server/mysql/data2
language   = /www/server/mysql/share/mysql/english
user       = unix_user1

[mysqld3]
mysqld     = /path/to/mysqld_safe
ledir      = /path/to/mysqld-binary/
mysqladmin = /path/to/mysqladmin
socket     = /tmp/mysql.sock3
port       = 3308
pid-file   = /www/server/mysql/data3/hostname.pid3
datadir    = /www/server/mysql/data3
language   = /www/server/mysql/share/mysql/swedish
user       = unix_user2

[mysqld4]
socket     = /tmp/mysql.sock4
port       = 3309
pid-file   = /www/server/mysql/data4/hostname.pid4
datadir    = /www/server/mysql/data4
language   = /www/server/mysql/share/mysql/estonia
user       = unix_user3

[mysqld6]
socket     = /tmp/mysql.sock6
port       = 3311
pid-file   = /www/server/mysql/data6/hostname.pid6
datadir    = /www/server/mysql/data6
language   = /www/server/mysql/share/mysql/japanese
user       = unix_user4

(base) [root@localhost ~]# mysqld_multi
mysqld_multi version 2.16 by Jani Tolonen

Description:
mysqld_multi can be used to start, reload, or stop any number of separate
mysqld processes running in different TCP/IP ports and UNIX sockets.

mysqld_multi can read group [mysqld_multi] from my.cnf file. You may
want to put options mysqld=... and mysqladmin=... there.  Since
version 2.10 these options can also be given under groups [mysqld#],
which gives more control over different versions.  One can have the
default mysqld and mysqladmin under group [mysqld_multi], but this is
not mandatory. Please note that if mysqld or mysqladmin is missing
from both [mysqld_multi] and [mysqld#], a group that is tried to be
used, mysqld_multi will abort with an error.

mysqld_multi will search for groups named [mysqld#] from my.cnf (or
the given --defaults-extra-file=...), where '#' can be any positive
integer starting from 1. These groups should be the same as the regular
[mysqld] group, but with those port, socket and any other options
that are to be used with each separate mysqld process. The number
in the group name has another function; it can be used for starting,
reloading, stopping, or reporting any specific mysqld server.

Usage: mysqld_multi [OPTIONS] {start|reload|stop|report} [GNR,GNR,GNR...]
or     mysqld_multi [OPTIONS] {start|reload|stop|report} [GNR-GNR,GNR,GNR-GNR,...]

The GNR means the group number. You can start, reload, stop or report any GNR,
or several of them at the same time. (See --example) The GNRs list can
be comma separated or a dash combined. The latter means that all the
GNRs between GNR1-GNR2 will be affected. Without GNR argument all the
groups found will either be started, reloaded, stopped, or reported. Note that
syntax for specifying GNRs must appear without spaces.

Options:

These options must be given before any others:
--no-defaults      Do not read any defaults file
--defaults-file=...  Read only this configuration file, do not read the
                   standard system-wide and user-specific files
--defaults-extra-file=...  Read this configuration file in addition to the
                   standard system-wide and user-specific files
Using:

--example          Give an example of a config file with extra information.
--help             Print this help and exit.
--log=...          Log file. Full path to and the name for the log file. NOTE:
                   If the file exists, everything will be appended.
                   Using: /www/server/data/mysqld_multi.log
--mysqladmin=...   mysqladmin binary to be used for a server shutdown.
                   Since version 2.10 this can be given within groups [mysqld#]
                   Using:
--mysqld=...       mysqld binary to be used. Note that you can give mysqld_safe
                   to this option also. The options are passed to mysqld. Just
                   make sure you have mysqld in your PATH or fix mysqld_safe.
                   Using:
                   Please note: Since mysqld_multi version 2.3 you can also
                   give this option inside groups [mysqld#] in ~/.my.cnf,
                   where '#' stands for an integer (number) of the group in
                   question. This will be recognised as a special option and
                   will not be passed to the mysqld. This will allow one to
                   start different mysqld versions with mysqld_multi.
--no-log           Print to stdout instead of the log file. By default the log
                   file is turned on.
--password=...     Password for mysqladmin user.
--silent           Disable warnings.
--tcp-ip           Connect to the MySQL server(s) via the TCP/IP port instead
                   of the UNIX socket. This affects stopping and reporting.
                   If a socket file is missing, the server may still be
                   running, but can be accessed only via the TCP/IP port.
                   By default connecting is done via the UNIX socket.
--user=...         mysqladmin user. Using: multi_admin
--verbose          Be more verbose.
--version          Print the version number and exit.
```



# innobackupex

```shell
innobackupex -help
Options:
    --apply-log
        Prepare a backup in BACKUP-DIR by applying the transaction log file
        named "xtrabackup_logfile" located in the same directory. Also,
        create new transaction logs. The InnoDB configuration is read from
        the file "backup-my.cnf".

    --compact
        Create a compact backup with all secondary index pages omitted. This
        option is passed directly to xtrabackup. See xtrabackup
        documentation for details.

    --compress
        This option instructs xtrabackup to compress backup copies of InnoDB
        data files. It is passed directly to the xtrabackup child process.
        Try 'xtrabackup --help' for more details.

    --compress-threads
        This option specifies the number of worker threads that will be used
        for parallel compression. It is passed directly to the xtrabackup
        child process. Try 'xtrabackup --help' for more details.

    --compress-chunk-size
        This option specifies the size of the internal working buffer for
        each compression thread, measured in bytes. It is passed directly to
        the xtrabackup child process. Try 'xtrabackup --help' for more
        details.

    --copy-back
        Copy all the files in a previously made backup from the backup
        directory to their original locations.

    --databases=LIST
        This option specifies the list of databases that innobackupex should
        back up. The option accepts a string argument or path to file that
        contains the list of databases to back up. The list is of the form
        "databasename1[.table_name1] databasename2[.table_name2] . . .". If
        this option is not specified, all databases containing MyISAM and
        InnoDB tables will be backed up. Please make sure that --databases
        contains all of the InnoDB databases and tables, so that all of the
        innodb.frm files are also backed up. In case the list is very long,
        this can be specified in a file, and the full path of the file can
        be specified instead of the list. (See option --tables-file.)

    --decompress
        Decompresses all files with the .qp extension in a backup previously
        made with the --compress option.

    --decrypt=ENCRYPTION-ALGORITHM
        Decrypts all files with the .xbcrypt extension in a backup
        previously made with --encrypt option.

    --debug-sleep-before-unlock=SECONDS
        This is a debug-only option used by the XtraBackup test suite.

    --defaults-file=[MY.CNF]
        This option specifies what file to read the default MySQL options
        from. The option accepts a string argument. It is also passed
        directly to xtrabackup's --defaults-file option. See the xtrabackup
        documentation for details.

    --defaults-group=GROUP-NAME
        This option specifies the group name in my.cnf which should be used.
        This is needed for mysqld_multi deployments.

    --defaults-extra-file=[MY.CNF]
        This option specifies what extra file to read the default MySQL
        options from before the standard defaults-file. The option accepts a
        string argument. It is also passed directly to xtrabackup's
        --defaults-extra-file option. See the xtrabackup documentation for
        details.

    --encrypt=ENCRYPTION-ALGORITHM
        This option instructs xtrabackup to encrypt backup copies of InnoDB
        data files using the algorithm specified in the
        ENCRYPTION-ALGORITHM. It is passed directly to the xtrabackup child
        process. Try 'xtrabackup --help' for more details.

    --encrypt-key=ENCRYPTION-KEY
        This option instructs xtrabackup to use the given ENCRYPTION-KEY
        when using the --encrypt or --decrypt options. During backup it is
        passed directly to the xtrabackup child process. Try 'xtrabackup
        --help' for more details.

    --encrypt-key-file=ENCRYPTION-KEY-FILE
        This option instructs xtrabackup to use the encryption key stored in
        the given ENCRYPTION-KEY-FILE when using the --encrypt or --decrypt
        options.

        Try 'xtrabackup --help' for more details.

    --encrypt-threads
        This option specifies the number of worker threads that will be used
        for parallel encryption. It is passed directly to the xtrabackup
        child process. Try 'xtrabackup --help' for more details.

    --encrypt-chunk-size
        This option specifies the size of the internal working buffer for
        each encryption thread, measured in bytes. It is passed directly to
        the xtrabackup child process. Try 'xtrabackup --help' for more
        details.

    --export
        This option is passed directly to xtrabackup's --export option. It
        enables exporting individual tables for import into another server.
        See the xtrabackup documentation for details.

    --extra-lsndir=DIRECTORY
        This option specifies the directory in which to save an extra copy
        of the "xtrabackup_checkpoints" file. The option accepts a string
        argument. It is passed directly to xtrabackup's --extra-lsndir
        option. See the xtrabackup documentation for details.

        ==item --force-non-empty-directories

        This option, when specified, makes --copy-back or --move-back
        transfer files to non-empty directories. Note that no existing files
        will be overwritten. If --copy-back or --nove-back has to copy a
        file from the backup directory which already exists in the
        destination directory, it will still fail with an error.

    --galera-info
        This options creates the xtrabackup_galera_info file which contains
        the local node state at the time of the backup. Option should be
        used when performing the backup of Percona-XtraDB-Cluster.

    --help
        This option displays a help screen and exits.

    --history=NAME
        This option enables the tracking of backup history in the
        PERCONA_SCHEMA.xtrabackup_history table. An optional history series
        name may be specified that will be placed with the history record
        for the current backup being taken.

    --host=HOST
        This option specifies the host to use when connecting to the
        database server with TCP/IP. The option accepts a string argument.
        It is passed to the mysql child process without alteration. See
        mysql --help for details.

    --ibbackup=IBBACKUP-BINARY
        This option specifies which xtrabackup binary should be used. The
        option accepts a string argument. IBBACKUP-BINARY should be the
        command used to run XtraBackup. The option can be useful if the
        xtrabackup binary is not in your search path or working directory.
        If this option is not specified, innobackupex attempts to determine
        the binary to use automatically. By default, "xtrabackup" is the
        command used. However, when option --copy-back is specified,
        "xtrabackup_51" is the command used. And when option --apply-log is
        specified, the binary is used whose name is in the file
        "xtrabackup_binary" in the backup directory, if that file exists.

    --include=REGEXP
        This option is a regular expression to be matched against table
        names in databasename.tablename format. It is passed directly to
        xtrabackup's --tables option. See the xtrabackup documentation for
        details.

    --incremental
        This option tells xtrabackup to create an incremental backup, rather
        than a full one. It is passed to the xtrabackup child process. When
        this option is specified, either --incremental-lsn or
        --incremental-basedir can also be given. If neither option is given,
        option --incremental-basedir is passed to xtrabackup by default, set
        to the first timestamped backup directory in the backup base
        directory.

    --incremental-basedir=DIRECTORY
        This option specifies the directory containing the full backup that
        is the base dataset for the incremental backup. The option accepts a
        string argument. It is used with the --incremental option.

    --incremental-dir=DIRECTORY
        This option specifies the directory where the incremental backup
        will be combined with the full backup to make a new full backup. The
        option accepts a string argument. It is used with the --incremental
        option.

    --incremental-history-name=NAME
        This option specifies the name of the backup series stored in the
        PERCONA_SCHEMA.xtrabackup_history history record to base an
        incremental backup on. Xtrabackup will search the history table
        looking for the most recent (highest innodb_to_lsn), successful
        backup in the series and take the to_lsn value to use as the
        starting lsn for the incremental backup. This will be mutually
        exclusive with --incremental-history-uuid, --incremental-basedir and
        --incremental-lsn. If no valid lsn can be found (no series by that
        name, no successful backups by that name) xtrabackup will return
        with an error. It is used with the --incremental option.

    --incremental-history-uuid=UUID
        This option specifies the UUID of the specific history record stored
        in the PERCONA_SCHEMA.xtrabackup_history to base an incremental
        backup on. --incremental-history-name, --incremental-basedir and
        --incremental-lsn. If no valid lsn can be found (no success record
        with that uuid) xtrabackup will return with an error. It is used
        with the --incremental option.

    --incremental-force-scan
        This options tells xtrabackup to perform full scan of data files for
        taking an incremental backup even if full changed page bitmap data
        is available to enable the backup without the full scan.

    --log-copy-interval
        This option specifies time interval between checks done by log
        copying thread in milliseconds.

    --incremental-lsn
        This option specifies the log sequence number (LSN) to use for the
        incremental backup. The option accepts a string argument. It is used
        with the --incremental option. It is used instead of specifying
        --incremental-basedir. For databases created by MySQL and Percona
        Server 5.0-series versions, specify the LSN as two 32-bit integers
        in high:low format. For databases created in 5.1 and later, specify
        the LSN as a single 64-bit integer.

    --kill-long-queries-timeout=SECONDS
        This option specifies the number of seconds innobackupex waits
        between starting FLUSH TABLES WITH READ LOCK and killing those
        queries that block it. Default is 0 seconds, which means
        innobackupex will not attempt to kill any queries.

    --kill-long-query-type=all|update
        This option specifies which types of queries should be killed to
        unblock the global lock. Default is "all".

    --lock-wait-timeout=SECONDS
        This option specifies time in seconds that innobackupex should wait
        for queries that would block FTWRL before running it. If there are
        still such queries when the timeout expires, innobackupex terminates
        with an error. Default is 0, in which case innobackupex does not
        wait for queries to complete and starts FTWRL immediately.

    --lock-wait-threshold=SECONDS
        This option specifies the query run time threshold which is used by
        innobackupex to detect long-running queries with a non-zero value of
        --lock-wait-timeout. FTWRL is not started until such long-running
        queries exist. This option has no effect if --lock-wait-timeout is
        0. Default value is 60 seconds.

    --lock-wait-query-type=all|update
        This option specifies which types of queries are allowed to complete
        before innobackupex will issue the global lock. Default is all.

    --move-back
        Move all the files in a previously made backup from the backup
        directory to the actual datadir location. Use with caution, as it
        removes backup files.

    --no-lock
        Use this option to disable table lock with "FLUSH TABLES WITH READ
        LOCK". Use it only if ALL your tables are InnoDB and you DO NOT CARE
        about the binary log position of the backup. This option shouldn't
        be used if there are any DDL statements being executed or if any
        updates are happening on non-InnoDB tables (this includes the system
        MyISAM tables in the mysql database), otherwise it could lead to an
        inconsistent backup. If you are considering to use --no-lock because
        your backups are failing to acquire the lock, this could be because
        of incoming replication events preventing the lock from succeeding.
        Please try using --safe-slave-backup to momentarily stop the
        replication slave thread, this may help the backup to succeed and
        you then don't need to resort to using this option.

    --no-timestamp
        This option prevents creation of a time-stamped subdirectory of the
        BACKUP-ROOT-DIR given on the command line. When it is specified, the
        backup is done in BACKUP-ROOT-DIR instead.

    --no-version-check
        This option disables the version check which is enabled by the
        --version-check option.

    --parallel=NUMBER-OF-THREADS
        On backup, this option specifies the number of threads the
        xtrabackup child process should use to back up files concurrently.
        The option accepts an integer argument. It is passed directly to
        xtrabackup's --parallel option. See the xtrabackup documentation for
        details.

        On --decrypt or --decompress it specifies the number of parallel
        forks that should be used to process the backup files.

    --password=WORD
        This option specifies the password to use when connecting to the
        database. It accepts a string argument. It is passed to the mysql
        child process without alteration. See mysql --help for details.

    --port=PORT
        This option specifies the port to use when connecting to the
        database server with TCP/IP. The option accepts a string argument.
        It is passed to the mysql child process. It is passed to the mysql
        child process without alteration. See mysql --help for details.

    --rebuild-indexes
        This option only has effect when used together with the --apply-log
        option and is passed directly to xtrabackup. When used, makes
        xtrabackup rebuild all secondary indexes after applying the log.
        This option is normally used to prepare compact backups. See the
        XtraBackup manual for more information.

    --rebuild-threads
        This option only has effect when used together with the --apply-log
        and --rebuild-indexes option and is passed directly to xtrabackup.
        When used, xtrabackup processes tablespaces in parallel with the
        specified number of threads when rebuilding indexes. See the
        XtraBackup manual for more information.

    --redo-only
        This option should be used when preparing the base full backup and
        when merging all incrementals except the last one. This option is
        passed directly to xtrabackup's --apply-log-only option. This forces
        xtrabackup to skip the "rollback" phase and do a "redo" only. This
        is necessary if the backup will have incremental changes applied to
        it later. See the xtrabackup documentation for details.

    --rsync
        Uses the rsync utility to optimize local file transfers. When this
        option is specified, innobackupex uses rsync to copy all non-InnoDB
        files instead of spawning a separate cp for each file, which can be
        much faster for servers with a large number of databases or tables.
        This option cannot be used together with --stream.

    --safe-slave-backup
        Stop slave SQL thread and wait to start backup until
        Slave_open_temp_tables in "SHOW STATUS" is zero. If there are no
        open temporary tables, the backup will take place, otherwise the SQL
        thread will be started and stopped until there are no open temporary
        tables. The backup will fail if Slave_open_temp_tables does not
        become zero after --safe-slave-backup-timeout seconds. The slave SQL
        thread will be restarted when the backup finishes.

    --safe-slave-backup-timeout
        How many seconds --safe-slave-backup should wait for
        Slave_open_temp_tables to become zero. (default 300)

    --slave-info
        This option is useful when backing up a replication slave server. It
        prints the binary log position and name of the master server. It
        also writes this information to the "xtrabackup_slave_info" file as
        a "CHANGE MASTER" command. A new slave for this master can be set up
        by starting a slave server on this backup and issuing a "CHANGE
        MASTER" command with the binary log position saved in the
        "xtrabackup_slave_info" file.

    --socket=SOCKET
        This option specifies the socket to use when connecting to the local
        database server with a UNIX domain socket. The option accepts a
        string argument. It is passed to the mysql child process without
        alteration. See mysql --help for details.

    --stream=STREAMNAME
        This option specifies the format in which to do the streamed backup.
        The option accepts a string argument. The backup will be done to
        STDOUT in the specified format. Currently, the only supported
        formats are tar and xbstream. This option is passed directly to
        xtrabackup's --stream option.

    --tables-file=FILE
        This option specifies the file in which there are a list of names of
        the form database. The option accepts a string argument.table, one
        per line. The option is passed directly to xtrabackup's
        --tables-file option.

    --throttle=IOS
        This option specifies a number of I/O operations (pairs of
        read+write) per second. It accepts an integer argument. It is passed
        directly to xtrabackup's --throttle option.

    --tmpdir=DIRECTORY
        This option specifies the location where a temporary file will be
        stored. The option accepts a string argument. It should be used when
        --stream is specified. For these options, the transaction log will
        first be stored to a temporary file, before streaming. This option
        specifies the location where that temporary file will be stored. If
        the option is not specified, the default is to use the value of
        tmpdir read from the server configuration.

    --use-memory=B
        This option accepts a string argument that specifies the amount of
        memory in bytes for xtrabackup to use for crash recovery while
        preparing a backup. Multiples are supported providing the unit (e.g.
        1MB, 1GB). It is used only with the option --apply-log. It is passed
        directly to xtrabackup's --use-memory option. See the xtrabackup
        documentation for details.

    --user=NAME
        This option specifies the MySQL username used when connecting to the
        server, if that's not the current user. The option accepts a string
        argument. It is passed to the mysql child process without
        alteration. See mysql --help for details.

    --version
        This option displays the xtrabackup version and copyright notice and
        then exits.

    --version-check
        This option controls if the version check should be executed by
        innobackupex after connecting to the server on the backup stage.
        This option is enabled by default, disable with --no-version-check.

[root@AY140715115907471c85Z backup]#
```



## 常用命令


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

      

   2. 22

   3. 22

   4. 

# Master-Slave 主从同步

## 现状

1. 174服务器上mysql3308 已经跑了N天，新建的mysql3308 是在192服务器。
2. 实现174服务器上mysql3308  数据 到 192服务器的mysql3308的数据同步，一主一备即可。

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
   
   
   # 执行 xtrabackup --prepare 
   
   # xtrabackup --prepare --host=127.0.0.1 --user=root --password='1db81ea576e76d9e' --port=3308 --target-dir=./x_backup
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
                     Master_Host: 192.168.1.174
                     Master_User: slave3308
                     Master_Port: 3308
                   Connect_Retry: 60
                 Master_Log_File: binlog.000003
             Read_Master_Log_Pos: 27444900
                  Relay_Log_File: localhost-relay-bin.000004
                   Relay_Log_Pos: 5576196
           Relay_Master_Log_File: binlog.000003
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
             Exec_Master_Log_Pos: 5576031
                 Relay_Log_Space: 27445278
                 Until_Condition: None
                  Until_Log_File:
                   Until_Log_Pos: 0
              Master_SSL_Allowed: No
              Master_SSL_CA_File:
              Master_SSL_CA_Path:
                 Master_SSL_Cert:
               Master_SSL_Cipher:
                  Master_SSL_Key:
           Seconds_Behind_Master: 3697
   Master_SSL_Verify_Server_Cert: No
                   Last_IO_Errno: 0
                   Last_IO_Error:
                  Last_SQL_Errno: 0
                  Last_SQL_Error:
     Replicate_Ignore_Server_Ids:
                Master_Server_Id: 3308
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
   ```

   

6. 至此，我们也完成了主库不停机加从库的操作。Oh Ye！

7. 

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



# ALTER

## ADD

```mysql
# 给已经存在的表并且非空 增加一个自增ID
ALTER TABLE tbl_name ADD id INT PRIMARY KEY AUTO_INCREMENT;
```

# pt-online-schema-change



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


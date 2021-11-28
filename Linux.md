# 常用命令

## rsync

```shell
rsync -av --progress ./ user@ip:/xxx/xxx/xxx/path
```

## awk

```shell
awk -help
Usage: awk [POSIX or GNU style options] -f progfile [--] file ...
Usage: awk [POSIX or GNU style options] [--] 'program' file ...
POSIX options:		GNU long options: (standard)
	-f progfile		--file=progfile
	-F fs			--field-separator=fs # 分隔符 比如nginx的access log ： awk -F \" '{print $6}' 按照双引号为分隔符分割
	-v var=val		--assign=var=val
Short options:		GNU long options: (extensions)
	-b			--characters-as-bytes
	-c			--traditional
	-C			--copyright
	-d[file]		--dump-variables[=file]
	-e 'program-text'	--source='program-text'
	-E file			--exec=file
	-g			--gen-pot
	-h			--help
	-L [fatal]		--lint[=fatal]
	-n			--non-decimal-data
	-N			--use-lc-numeric
	-O			--optimize
	-p[file]		--profile[=file]
	-P			--posix
	-r			--re-interval
	-S			--sandbox
	-t			--lint-old
	-V			--version

To report bugs, see node `Bugs' in `gawk.info', which is
section `Reporting Problems and Bugs' in the printed version.

gawk is a pattern scanning and processing language.
By default it reads standard input and writes standard output.

Examples:
	gawk '{ sum += $1 }; END { print sum }' file
	gawk -F: '{ print $1 }' /etc/passwd
	
	
```

### 小练习

1. Nginx的web服务器日志 access log文件，需要从统计日志中统计有哪些类型的设备终端和浏览器访问了该网站

   ````shell
   #1. 先解析出来所有的 user-agent
   # 处理该文件的每一行，指定 " 为分隔符，只输出第6个文本域
   cat /www/wwwlogs/local.web.access.log | awk -F \" '{print $6}' > ua.txt  
   
   ```
   Mozilla/5.0 (Linux; U; Android 9; zh-CN; MI 8 Build/PKQ1.180729.001) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/57.0.2987.108 UCBrowser/12.8.9.1069 Mobi
   le Safari/537.36
   Mozilla/5.0 (Linux; U; Android 9; zh-CN; MI 8 Build/PKQ1.180729.001) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/57.0.2987.108 UCBrowser/12.8.9.1069 Mobi
   le Safari/537.36
   Mozilla/5.0 (Linux; U; Android 9; zh-CN; MI 8 Build/PKQ1.180729.001) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/57.0.2987.108 UCBrowser/12.8.9.1069 Mobi
   le Safari/537.36
   Mozilla/5.0 (Linux; U; Android 9; zh-CN; MI 8 Build/PKQ1.180729.001) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/57.0.2987.108 UCBrowser/12.8.9.1069 Mobi
   le Safari/537.36
   Mozilla/5.0 (Linux; U; Android 9; zh-CN; MI 8 Build/PKQ1.180729.001) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/57.0.2987.108 UCBrowser/12.8.9.1069 Mobi
   le Safari/537.36
   Mozilla/5.0 (Linux; U; Android 9; zh-CN; MI 8 Build/PKQ1.180729.001) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/57.0.2987.108 UCBrowser/12.8.9.1069 Mobi
   le Safari/537.36
   Dalvik/2.1.0 (Linux; U; Android 9.0; ZTE BA520 Build/MRA58K)
   ```
   
   #2. 将user-agent 做唯一值处理
   sort ua.txt | uniq -u > ua1.txt
   
   # 也可以直接通过管道命令：
   cat  /www/wwwlogs/local.web.access.log| awk -F \" '{print $6}' | sort | uniq -c | sort -nr
   ````

   

# ELK

## 安装

## Filebeat

### 开启mysql模块

https://help.aliyun.com/document_detail/170479.html

```shell
> filebeat modules enable mysql
> vim /etc/filebeat/modules.d/mysql.yml

var.paths: ["/www/server/data/mysql-slow.log","/data2/www/server/mysqldata/data3308/mysql-slow.log"] # 打开

> service filebeat restart
```



### 启动命令

```shell
systemctl restart elasticsearch.service
systemctl restart filebeat.service
```



## kibana-keystore

```shell
cd /usr/share/kibana/bin/
./bin/kibana-keystore list

# Unable to create actions client because the Encrypted Saved Objects plugin is missing encryption key. Please set xpack.encryptedSavedObjects.encryptionKey in the kibana.yml or use the bin/kibana-encryption-keys command.
# 解决这个问题，就是在kibana.yml 文件新增 xpack.encryptedSavedObjects.encryptionKey。kibana为了方便，提供了 kibana-keystore 工具，可以自动添加，add后 不需要在kibana.yml 中再次添加。
./bin/kibana-keystore add xpack.encryptedSavedObjects.encryptionKey
Enter value for xpack.encryptedSavedObjects.encryptionKey: ********************************

E1XL3pvgwYyL47p7YKZwj4aNvxoSSiuE

./bin/kibana-keystore add  elasticsearch.username
Enter value for elasticsearch.username: ******* 

./bin/kibana-keystore add  elasticsearch.password
Enter value for elasticsearch.password: *******

./bin/kibana-keystore list
xpack.encryptedSavedObjects.encryptionKey
elasticsearch.username
elasticsearch.password
```



## 安全

https://www.elastic.co/guide/en/kibana/current/using-kibana-with-security.html

https://www.elastic.co/guide/en/kibana/7.9/alert-action-settings-kb.html#general-alert-action-settings



## 常见问题

1. Unable to create actions client because the Encrypted Saved Objects plugin is missing encryption key. Please set xpack.encryptedSavedObjects.encryptionKey in the kibana.yml or use the bin/kibana-encryption-keys command.

2. exception: Security must be explicitly enabled when using a [basic] license. Enable security by setting [xpack.security.enabled] to [true] in the elasticsearch.yml file and restart the node. (500)

3. License is not available.（es的用户名密码校验不通过）

   ```json
   {"statusCode":503,"error":"Service Unavailable","message":"License is not available."}
   ```

   

4. 日志

   ```shell
    tail -f /var/log/kibana/kibana.log
   {"type":"log","@timestamp":"2021-11-17T16:19:16+08:00","tags":["warning","plugins","licensing"],"pid":2935,"message":"License information could not be obtained from Elasticsearch due to ConnectionError: connect ECONNREFUSED 127.0.0.1:9200 error"}
   ```

   

5. xx

6. xx

# SSH免密登录

SSH免密登录一般可以采用下面三种方式，看个人习惯。

## ① ssh-copy-id

```shell
ssh-copy-id -i ~/.ssh/id_rsa.pub root@xx.xx.xx.xx  # 然后输入对应的密码即可。
```

## ② scp

```shell
# 通过 scp 命令直接将该文件远程复制过去，使用这种方式需要注意，如果你之前已经配置了其它服务器上的密钥，这是使用这种方法，就会覆盖掉你原来的密钥，这时候是不建议使用这种方式的，如果你是先将该文件复制到服务器上的一个目录下，然后在使用追加的方式，将密钥追加到 authorized_keys 也是完全 OK 的。
scp -p ~/.ssh/id_rsa.pub root@:/root/.ssh/authorized_keys
```

## ③ 纯手工复制

通过手工复制。将本地 id_rsa.pub 文件的内容拷贝至远程服务器的 ~/.ssh/authorized_keys 文件中也完全可以的。

先使用 cat 命令查看当前的公钥，然后复制，在到目标服务器上去粘贴。

### 别名配置

```shell
vim ~/.ssh/config
# 添加以下内容
Host server_name
        HostName xxx.xxx.xxx.xxx
        User root
        Port 22
        
# 保存退出

ssh server_name # 即可直接登入服务器。方便很多。
```









# 常见问题

## ssh 登录会话保持时间太短解决方法：

```shell
vim /etc/ssh/sshd_config

ClientAliveInterval 60
ClientAliveCountMax 3

service sshd reload 重启服务
```

## 内网IP配置

```shell
# ip addr 查看网卡
vim /etc/sysconfig/network-scripts/ifcfg-enp5s0f1

TYPE=Ethernet
BOOTPROTO=dhcp # 改为 static
DEFROUTE=yes
PEERDNS=yes
PEERROUTES=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
IPV6_FAILURE_FATAL=no
NAME=enp5s0f1
UUID=91a7771b-9c71-418f-bf3e-eede0d1f54c6
DEVICE=enp5s0f1
ONBOOT=no  # yes

IPADDR=192.168.1.192 #内网IP 地址
METMASK=255.255.255.0 
GATEWAY=192.168.1.1 

# 最后一步 重启网卡
service network restart 
```



# BT

## 安装

参考文档：https://www.bt.cn/bbs/thread-19376-1-1.html

```shell
# yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh

```


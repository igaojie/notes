# ELK

## 安装

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

7. 

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


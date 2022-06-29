# 安装 

## docker 安装

### 安装

````shell
# pull 镜像
> docker pull docker.elastic.co/elasticsearch/elasticsearch:7.15.2

```
7.15.2: Pulling from elasticsearch/elasticsearch
009c11f4ddee: Pull complete
8772b99d888d: Pull complete
bd8b744bf3bf: Pull complete
2a41be2c565a: Pull complete
e7e9200dd33e: Pull complete
Digest: sha256:a1dce08d504b22e87adc849c94dcae53f6a0bd12648a4d99d7f9fc07bb2e8a3e
Status: Downloaded newer image for docker.elastic.co/elasticsearch/elasticsearch:7.15.2
docker.elastic.co/elasticsearch/elasticsearch:7.15.2
```

# 启动 es 服务
> docker run -p 127.0.0.1:9200:9200 -p 127.0.0.1:9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.15.2
````

### 校验

```shell
docker exec -it 95ac431a5b87ae109bf04dfe8f3adbcede8a7209212eb7bedba85a4e092bc9d7 /bin/sh

sh-4.4# curl -X GET "localhost:9200/?pretty"
{
  "name" : "95ac431a5b87",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "ny4n5hrzQpS5ozYO6RhQHA",
  "version" : {
    "number" : "7.15.2",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "93d5a7f6192e8a1a12e154a2b81bf6fa7309da0c",
    "build_date" : "2021-11-04T14:04:42.515624022Z",
    "build_snapshot" : false,
    "lucene_version" : "8.9.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}

# ok 安装完成 可以继续了。
```



## Centos YUM安装

1、在 /etc/yum.repos.d/ 目录下新建 elasticsearch.repo 内容如下：

```shell
[elasticsearch]
name=Elasticsearch repository for 8.x packages
baseurl=https://artifacts.elastic.co/packages/8.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=0
autorefresh=1
type=rpm-md
```

2、安装

```shell
sudo yum install --enablerepo=elasticsearch elasticsearch


sudo sudo yum install --enablerepo=elasticsearch elasticsearch
Elasticsearch repository for 8.x packages                               2.5 MB/s | 9.0 MB     00:03
上次元数据过期检查：0:00:05 前，执行于 2022年06月02日 星期四 15时12分44秒。
依赖关系解决。
========================================================================================================
 软件包                     架构                版本                   仓库                        大小
========================================================================================================
安装:
 elasticsearch              x86_64              8.2.2-1                elasticsearch              502 M

事务概要
========================================================================================================
安装  1 软件包

总下载：502 M
安装大小：1.0 G
确定吗？[y/N]： y
下载软件包：
elasticsearch-8.2.2-x86_64.rpm                                          5.4 MB/s | 502 MB     01:33
--------------------------------------------------------------------------------------------------------
总计                                                                    5.4 MB/s | 502 MB     01:33
警告：/var/cache/dnf/elasticsearch-12170fef1b21a4ec/packages/elasticsearch-8.2.2-x86_64.rpm: 头V4 RSA/SHA512 Signature, 密钥 ID d88e42b4: NOKEY
Elasticsearch repository for 8.x packages                               5.3 kB/s | 1.7 kB     00:00
导入 GPG 公钥 0xD88E42B4:
 Userid: "Elasticsearch (Elasticsearch Signing Key) <dev_ops@elasticsearch.org>"
 指纹: 4609 5ACC 8548 582C 1A26 99A9 D27D 666C D88E 42B4
 来自: https://artifacts.elastic.co/GPG-KEY-elasticsearch
确定吗？[y/N]： y
导入公钥成功
运行事务检查
事务检查成功。
运行事务测试
事务测试成功。
运行事务
  准备中  :                                                                                                            1/1
  运行脚本: elasticsearch-8.2.2-1.x86_64                                                                               1/1
Creating elasticsearch group... OK
Creating elasticsearch user... OK

  安装    : elasticsearch-8.2.2-1.x86_64                                                                               1/1
  运行脚本: elasticsearch-8.2.2-1.x86_64                                                                               1/1
--------------------------- Security autoconfiguration information ------------------------------

Authentication and authorization are enabled.
TLS for the transport and HTTP layers is enabled and configured.

The generated password for the elastic built-in superuser is : IDahZlKsGpn_Gi1Hnvec

If this node should join an existing cluster, you can reconfigure this with
'/usr/share/elasticsearch/bin/elasticsearch-reconfigure-node --enrollment-token <token-here>'
after creating an enrollment token on your existing cluster.

You can complete the following actions at any time:

Reset the password of the elastic built-in superuser with
'/usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic'.

Generate an enrollment token for Kibana instances with
 '/usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana'.

Generate an enrollment token for Elasticsearch nodes with
'/usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s node'.

-------------------------------------------------------------------------------------------------
### NOT starting on installation, please execute the following statements to configure elasticsearch service to start automatically using systemd
 sudo systemctl daemon-reload
 sudo systemctl enable elasticsearch.service
### You can start elasticsearch service by executing
 sudo systemctl start elasticsearch.service

Couldn't write '0' to 'kernel/yama/ptrace_scope', ignoring: No such file or directory

[/usr/lib/tmpfiles.d/elasticsearch.conf:1] Line references path below legacy directory /var/run/, updating /var/run/elasticsearch → /run/elasticsearch; please update the tmpfiles.d/ drop-in file accordingly.

  验证    : elasticsearch-8.2.2-1.x86_64                                                                               1/1

已安装:
  elasticsearch-8.2.2-1.x86_64

完毕！
[root@xinqiao yum.repos.d]#

# 修改密码
/usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
This tool will reset the password of the [elastic] user to an autogenerated value.
The password will be printed in the console.
Please confirm that you would like to continue [y/N]y


Password for the [elastic] user successfully reset.
New value: BgSoQg4EzrzZmfvfN0rD
[root@xinqiao software]#
[root@xinqiao software]#
[root@xinqiao software]# curl -u elastic:BgSoQg4EzrzZmfvfN0rD http://localhost:9200/
{
  "name" : "xinqiao",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "hHlLX3o9RM-WWyt3n0XVKw",
  "version" : {
    "number" : "8.2.2",
    "build_flavor" : "default",
    "build_type" : "rpm",
    "build_hash" : "9876968ef3c745186b94fdabd4483e01499224ef",
    "build_date" : "2022-05-25T15:47:06.259735307Z",
    "build_snapshot" : false,
    "lucene_version" : "9.1.0",
    "minimum_wire_compatibility_version" : "7.17.0",
    "minimum_index_compatibility_version" : "7.0.0"
  },
  "tagline" : "You Know, for Search"
}


```

3、配置开机启动

```shell
sudo /bin/systemctl daemon-reload
sudo /bin/systemctl enable elasticsearch.service
#启动 es 服务
sudo systemctl start elasticsearch.service
#停止 es服务
sudo systemctl stop elasticsearch.service

```

4、修改data.path 到数据盘 （切记要将权限修改为 elasticsearch:elasticsearch）

## 安装包安装



# RPM 安装

```she
 # 由于ik分词器还不兼容8.2.2版本 所以 重新装 8.2.0版本
 wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.2.0-x86_64.rpm
 
 
 curl -u elastic:changeme http://localhost:9200/
{
  "name" : "xinqiao",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "dgVIo5M0TVSvSoKOR-_vtg",
  "version" : {
    "number" : "8.2.0",
    "build_flavor" : "default",
    "build_type" : "rpm",
    "build_hash" : "b174af62e8dd9f4ac4d25875e9381ffe2b9282c5",
    "build_date" : "2022-04-20T10:35:10.180408517Z",
    "build_snapshot" : false,
    "lucene_version" : "9.1.0",
    "minimum_wire_compatibility_version" : "7.17.0",
    "minimum_index_compatibility_version" : "7.0.0"
  },
  "tagline" : "You Know, for Search"
}

# ik 包
cd /usr/share/elasticsearch/
[root@xinqiao elasticsearch]# ./bin/elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v8.2.0/elasticsearch-analysis-ik-8.2.0.zip
```



# cerebro 可视化管理工具

## 安装

```shell
 # 1. wget https://github.com/lmenezes/cerebro/releases/download/v0.9.4/cerebro-0.9.4-1.noarch.rpm
 
 # 2. rpm -ivh cerebro-0.9.4-1.noarch.rpm
Verifying...                          ################################# [100%]
准备中...                          ################################# [100%]
Creating system group: cerebro
Creating system user: cerebro in cerebro with cerebro user-daemon and shell /bin/false
正在升级/安装...
   1:cerebro-0.9.4-1                  ################################# [100%]
Created symlink /etc/systemd/system/multi-user.target.wants/cerebro.service → /usr/lib/systemd/system/cerebro.service.
 
```



# 安全

## SQL

https://www.elastic.co/guide/en/elasticsearch/reference/master/sql-cli.html

```shell
> cd /usr/share/elasticsearch/bin/
# ./elasticsearch-sql-cli http://用户名：密码@host:port
> ./elasticsearch-sql-cli http://sql_user:strongpassword@192.168.1.174:9200 

WARNING: sun.reflect.Reflection.getCallerClass is not supported. This will impact performance.
                       asticElasticE
                     ElasticE  sticEla
          sticEl  ticEl            Elast
        lasti Elasti                   tic
      cEl       ast                     icE
     icE        as                       cEl
     icE        as                       cEl
     icEla     las                        El
   sticElasticElast                     icElas
 las           last                    ticElast
El              asti                 asti    stic
El              asticEla           Elas        icE
El            Elas  cElasticE   ticEl           cE
Ela        ticEl         ticElasti              cE
 las     astic               last              icE
   sticElas                   asti           stic
     icEl                      sticElasticElast
     icE                       sticE   ticEla
     icE                       sti       cEla
     icEl                      sti        Ela
      cEl                      sti       cEl
       Ela                    astic    ticE
         asti               ElasticElasti
           ticElasti  lasticElas
              ElasticElast

                       SQL
                      7.15.0

sql>



```

### show tables

```shell

# sql> show tables; 列出所有的索引库 
sql> show tables;
               name                |     type      |     kind
-----------------------------------+---------------+---------------
.apm-agent-configuration           |TABLE          |INDEX
.apm-custom-link                   |TABLE          |INDEX
.async-search                      |TABLE          |INDEX
.kibana                            |VIEW           |ALIAS
.kibana-event-log-7.14.1           |VIEW           |ALIAS
.kibana-event-log-7.14.1-000001    |TABLE          |INDEX
.kibana-event-log-7.14.1-000002    |TABLE          |INDEX
.kibana-event-log-7.14.1-000003    |TABLE          |INDEX
.kibana_7.14.1                     |VIEW           |ALIAS
.kibana_7.14.1_001                 |TABLE          |INDEX
.kibana_task_manager               |VIEW           |ALIAS
.kibana_task_manager_7.14.1        |VIEW           |ALIAS
.kibana_task_manager_7.14.1_001    |TABLE          |INDEX
.monitoring-alerts-7               |TABLE          |INDEX
.monitoring-es-7-2021.11.20        |TABLE          |INDEX
.monitoring-es-7-2021.11.21        |TABLE          |INDEX
.monitoring-es-7-2021.11.22        |TABLE          |INDEX
.monitoring-es-7-2021.11.23        |TABLE          |INDEX
.monitoring-es-7-2021.11.24        |TABLE          |INDEX
.monitoring-es-7-2021.11.25        |TABLE          |INDEX
.monitoring-es-7-2021.11.26        |TABLE          |INDEX
.monitoring-kibana-7-2021.11.20    |TABLE          |INDEX
.monitoring-kibana-7-2021.11.21    |TABLE          |INDEX
.monitoring-kibana-7-2021.11.22    |TABLE          |INDEX
.monitoring-kibana-7-2021.11.23    |TABLE          |INDEX
.monitoring-kibana-7-2021.11.24    |TABLE          |INDEX
.monitoring-kibana-7-2021.11.25    |TABLE          |INDEX
.monitoring-kibana-7-2021.11.26    |TABLE          |INDEX
.tasks                             |TABLE          |INDEX
.triggered_watches                 |TABLE          |INDEX
.watches                           |TABLE          |INDEX
article_post                       |TABLE          |INDEX
filebeat-7.14.1                    |VIEW           |ALIAS
filebeat-7.14.1-2021.09.11-000001  |TABLE          |INDEX
filebeat-7.14.1-2021.10.11-000002  |TABLE          |INDEX
filebeat-7.14.1-2021.11.10-000003  |TABLE          |INDEX
filebeat-7.15.0                    |VIEW           |ALIAS
filebeat-7.15.0-2021.10.11-000001  |TABLE          |INDEX
filebeat-7.15.0-2021.11.10-000002  |TABLE          |INDEX
index                              |TABLE          |INDEX
metricbeat-7.14.1                  |VIEW           |ALIAS
metricbeat-7.14.1-2021.09.17-000001|TABLE          |INDEX
metricbeat-7.14.1-2021.10.17-000002|TABLE          |INDEX
metricbeat-7.14.1-2021.11.16-000003|TABLE          |INDEX
metricbeat-7.15.0                  |VIEW           |ALIAS
metricbeat-7.15.0-2021.11.17-000001|TABLE          |INDEX

sql>
```

### show functions

```shell
sql> SHOW FUNCTIONS;
      name       |     type
-----------------+---------------
AVG              |AGGREGATE
COUNT            |AGGREGATE
FIRST            |AGGREGATE
FIRST_VALUE      |AGGREGATE
LAST             |AGGREGATE
LAST_VALUE       |AGGREGATE
MAX              |AGGREGATE
MIN              |AGGREGATE
SUM              |AGGREGATE
KURTOSIS         |AGGREGATE
MAD              |AGGREGATE
PERCENTILE       |AGGREGATE
PERCENTILE_RANK  |AGGREGATE
SKEWNESS         |AGGREGATE
STDDEV_POP       |AGGREGATE
STDDEV_SAMP      |AGGREGATE
SUM_OF_SQUARES   |AGGREGATE
VAR_POP          |AGGREGATE
VAR_SAMP         |AGGREGATE
HISTOGRAM        |GROUPING
CASE             |CONDITIONAL
COALESCE         |CONDITIONAL
GREATEST         |CONDITIONAL
IFNULL           |CONDITIONAL
IIF              |CONDITIONAL
ISNULL           |CONDITIONAL
LEAST            |CONDITIONAL
NULLIF           |CONDITIONAL
NVL              |CONDITIONAL
CURDATE          |SCALAR
CURRENT_DATE     |SCALAR
CURRENT_TIME     |SCALAR
CURRENT_TIMESTAMP|SCALAR
CURTIME          |SCALAR
DATEADD          |SCALAR
DATEDIFF         |SCALAR
DATEPART         |SCALAR
DATETIME_FORMAT  |SCALAR
DATETIME_PARSE   |SCALAR
DATETRUNC        |SCALAR
DATE_ADD         |SCALAR
DATE_DIFF        |SCALAR
DATE_PARSE       |SCALAR
DATE_PART        |SCALAR
DATE_TRUNC       |SCALAR
DAY              |SCALAR
DAYNAME          |SCALAR
DAYOFMONTH       |SCALAR
DAYOFWEEK        |SCALAR
DAYOFYEAR        |SCALAR
DAY_NAME         |SCALAR
DAY_OF_MONTH     |SCALAR
DAY_OF_WEEK      |SCALAR
DAY_OF_YEAR      |SCALAR
DOM              |SCALAR
DOW              |SCALAR
DOY              |SCALAR
FORMAT           |SCALAR
HOUR             |SCALAR
HOUR_OF_DAY      |SCALAR
IDOW             |SCALAR
ISODAYOFWEEK     |SCALAR
ISODOW           |SCALAR
ISOWEEK          |SCALAR
ISOWEEKOFYEAR    |SCALAR
ISO_DAY_OF_WEEK  |SCALAR
ISO_WEEK_OF_YEAR |SCALAR
IW               |SCALAR
IWOY             |SCALAR
MINUTE           |SCALAR
MINUTE_OF_DAY    |SCALAR
MINUTE_OF_HOUR   |SCALAR
MONTH            |SCALAR
MONTHNAME        |SCALAR
MONTH_NAME       |SCALAR
MONTH_OF_YEAR    |SCALAR
NOW              |SCALAR
QUARTER          |SCALAR
SECOND           |SCALAR
SECOND_OF_MINUTE |SCALAR
TIMESTAMPADD     |SCALAR
TIMESTAMPDIFF    |SCALAR
TIMESTAMP_ADD    |SCALAR
TIMESTAMP_DIFF   |SCALAR
TIME_PARSE       |SCALAR
TODAY            |SCALAR
TO_CHAR          |SCALAR
WEEK             |SCALAR
WEEK_OF_YEAR     |SCALAR
YEAR             |SCALAR
ABS              |SCALAR
ACOS             |SCALAR
ASIN             |SCALAR
ATAN             |SCALAR
ATAN2            |SCALAR
CBRT             |SCALAR
CEIL             |SCALAR
CEILING          |SCALAR
COS              |SCALAR
COSH             |SCALAR
COT              |SCALAR
DEGREES          |SCALAR
E                |SCALAR
EXP              |SCALAR
EXPM1            |SCALAR
FLOOR            |SCALAR
LOG              |SCALAR
LOG10            |SCALAR
MOD              |SCALAR
PI               |SCALAR
POWER            |SCALAR
RADIANS          |SCALAR
RAND             |SCALAR
RANDOM           |SCALAR
ROUND            |SCALAR
SIGN             |SCALAR
SIGNUM           |SCALAR
SIN              |SCALAR
SINH             |SCALAR
SQRT             |SCALAR
TAN              |SCALAR
TRUNC            |SCALAR
TRUNCATE         |SCALAR
ASCII            |SCALAR
BIT_LENGTH       |SCALAR
CHAR             |SCALAR
CHARACTER_LENGTH |SCALAR
CHAR_LENGTH      |SCALAR
CONCAT           |SCALAR
INSERT           |SCALAR
LCASE            |SCALAR
LEFT             |SCALAR
LENGTH           |SCALAR
LOCATE           |SCALAR
LTRIM            |SCALAR
OCTET_LENGTH     |SCALAR
POSITION         |SCALAR
REPEAT           |SCALAR
REPLACE          |SCALAR
RIGHT            |SCALAR
RTRIM            |SCALAR
SPACE            |SCALAR
STARTS_WITH      |SCALAR
SUBSTRING        |SCALAR
TRIM             |SCALAR
UCASE            |SCALAR
CAST             |SCALAR
CONVERT          |SCALAR
DATABASE         |SCALAR
USER             |SCALAR
ST_ASTEXT        |SCALAR
ST_ASWKT         |SCALAR
ST_DISTANCE      |SCALAR
ST_GEOMETRYTYPE  |SCALAR
ST_GEOMFROMTEXT  |SCALAR
ST_WKTTOSQL      |SCALAR
ST_X             |SCALAR
ST_Y             |SCALAR
ST_Z             |SCALAR
SCORE            |SCORE

sql>

sql>
   |
   | SHOW FUNCTIONS LIKE 'ABS';
     name      |     type
---------------+---------------
ABS            |SCALAR
```



### SELECT

https://www.elastic.co/guide/en/elasticsearch/reference/master/sql-syntax-select.html

```shell
# select 语句 ，类似SQL
sql> select postid, title, subtitle, platform, mid  from article_post limit 1;
    
    postid     |       title        |   subtitle    |   platform    |      mid
---------------+--------------------+---------------+---------------+---------------
117606         |6TY Shades of Beauty|               |1              |0
```

### DESCRIBE

```shell
sql>
   |
   | describe article_post;  # desc article_post;
     column      |     type      |    mapping
-----------------+---------------+---------------
content          |VARCHAR        |text
desc             |VARCHAR        |text
mid              |TINYINT        |byte
platform         |TINYINT        |byte
postid           |INTEGER        |integer
subtitle         |VARCHAR        |text
subtitle:        |VARCHAR        |text
subtitle:.keyword|VARCHAR        |keyword
title            |VARCHAR        |text
versionContent   |VARCHAR        |text



sql>
   |
   | describe "filebeat-*"
```



# 分词器

## 类型

### simple

```shell
POST _analyze
{
  "analyzer": "simple",
  "text":     "Like X 国庆放假 的"
}

{
  "tokens" : [
    {
      "token" : "like",
      "start_offset" : 0,
      "end_offset" : 4,
      "type" : "word",
      "position" : 0
    },
    {
      "token" : "x",
      "start_offset" : 5,
      "end_offset" : 6,
      "type" : "word",
      "position" : 1
    },
    {
      "token" : "国庆放假",
      "start_offset" : 7,
      "end_offset" : 11,
      "type" : "word",
      "position" : 2
    },
    {
      "token" : "的",
      "start_offset" : 12,
      "end_offset" : 13,
      "type" : "word",
      "position" : 3
    }
  ]
}

```

### whitespace

`whitespace` （空白字符）分词器按空白字符 —— 空格、tabs、换行符等等进行简单拆分 —— 然后假定连续的非空格字符组成了一个语汇单元

```shell
POST _analyze
{
  "analyzer": "whitespace",
  "text":     "Like X 国庆放假的"
}

{
  "tokens" : [
    {
      "token" : "Like",
      "start_offset" : 0,
      "end_offset" : 4,
      "type" : "word",
      "position" : 0
    },
    {
      "token" : "X",
      "start_offset" : 5,
      "end_offset" : 6,
      "type" : "word",
      "position" : 1
    },
    {
      "token" : "国庆放假的",
      "start_offset" : 7,
      "end_offset" : 12,
      "type" : "word",
      "position" : 2
    }
  ]
}

```

### Standard

```shell
POST _analyze
{
  "analyzer": "standard",
  "text":     "Like X 国庆放假的"
}

{
  "tokens" : [
    {
      "token" : "like",
      "start_offset" : 0,
      "end_offset" : 4,
      "type" : "<ALPHANUM>",
      "position" : 0
    },
    {
      "token" : "x",
      "start_offset" : 5,
      "end_offset" : 6,
      "type" : "<ALPHANUM>",
      "position" : 1
    },
    {
      "token" : "国",
      "start_offset" : 7,
      "end_offset" : 8,
      "type" : "<IDEOGRAPHIC>",
      "position" : 2
    },
    {
      "token" : "庆",
      "start_offset" : 8,
      "end_offset" : 9,
      "type" : "<IDEOGRAPHIC>",
      "position" : 3
    },
    {
      "token" : "放",
      "start_offset" : 9,
      "end_offset" : 10,
      "type" : "<IDEOGRAPHIC>",
      "position" : 4
    },
    {
      "token" : "假",
      "start_offset" : 10,
      "end_offset" : 11,
      "type" : "<IDEOGRAPHIC>",
      "position" : 5
    },
    {
      "token" : "的",
      "start_offset" : 11,
      "end_offset" : 12,
      "type" : "<IDEOGRAPHIC>",
      "position" : 6
    }
  ]
}

```



### ik

项目地址：https://github.com/medcl/elasticsearch-analysis-ik 

#### 安装

查看当前使用的es版本号：

```shell
curl -XGET http://localhost:9200/
{
  "name" : "localhost.localdomain",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "FMy41HX-Tlu0ew8KSL3ymQ",
  "version" : {
    "number" : "7.15.0", # 根据官网说明 可以使用IK version	：master版本
    "build_flavor" : "default",
    "build_type" : "rpm",
    "build_hash" : "79d65f6e357953a5b3cbcc5e2c7c21073d89aa29",
    "build_date" : "2021-09-16T03:05:29.143308416Z",
    "build_snapshot" : false,
    "lucene_version" : "8.9.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}


 cd /usr/share/elasticsearch/
 # 执行安装命令 一定要注意版本 
 ./bin/elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.15.0/elasticsearch-analysis-ik-7.15.0.zip
 
 -> Installing https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.15.0/elasticsearch-analysis-ik-7.15.0.zip
-> Downloading https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.15.0/elasticsearch-analysis-ik-7.15.0.zip
[=================================================] 100%
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@     WARNING: plugin requires additional permissions     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
* java.net.SocketPermission * connect,resolve
See http://docs.oracle.com/javase/8/docs/technotes/guides/security/permissions.html
for descriptions of what these permissions allow and the associated risks.

Continue with installation? [y/N]y
-> Installed analysis-ik
-> Please restart Elasticsearch to activate any plugins installed
(base) [root@localhost elasticsearch]#  systemctl restart elasticsearch.service 重启服务

```

#### 验证

`ik_max_word` 会将文本做最细粒度的拆分，比如会将“中华人民共和国国歌”拆分为“中华人民共和国,中华人民,中华,华人,人民共和国,人民,人,民,共和国,共和,和,国国,国歌”，会穷尽各种可能的组合，适合 Term Query；

`ik_smart` 会做最粗粒度的拆分，比如会将“中华人民共和国国歌”拆分为“中华人民共和国,国歌”，适合 Phrase 查询。

两种分词器使用的最佳实践是：索引时用ik_max_word，在搜索时用ik_smart。索引时最大化的将文章内容分词，搜索时更精确的搜索到想要的结果。

## 

```shell
GET /_analyze
{
  "text":"中华人民共和国国徽",
  "analyzer":"ik_max_word"
}

{
  "tokens" : [
    {
      "token" : "中华人民共和国",
      "start_offset" : 0,
      "end_offset" : 7,
      "type" : "CN_WORD",
      "position" : 0
    },
    {
      "token" : "中华人民",
      "start_offset" : 0,
      "end_offset" : 4,
      "type" : "CN_WORD",
      "position" : 1
    },
    {
      "token" : "中华",
      "start_offset" : 0,
      "end_offset" : 2,
      "type" : "CN_WORD",
      "position" : 2
    },
    {
      "token" : "华人",
      "start_offset" : 1,
      "end_offset" : 3,
      "type" : "CN_WORD",
      "position" : 3
    },
    {
      "token" : "人民共和国",
      "start_offset" : 2,
      "end_offset" : 7,
      "type" : "CN_WORD",
      "position" : 4
    },
    {
      "token" : "人民",
      "start_offset" : 2,
      "end_offset" : 4,
      "type" : "CN_WORD",
      "position" : 5
    },
    {
      "token" : "共和国",
      "start_offset" : 4,
      "end_offset" : 7,
      "type" : "CN_WORD",
      "position" : 6
    },
    {
      "token" : "共和",
      "start_offset" : 4,
      "end_offset" : 6,
      "type" : "CN_WORD",
      "position" : 7
    },
    {
      "token" : "国",
      "start_offset" : 6,
      "end_offset" : 7,
      "type" : "CN_CHAR",
      "position" : 8
    },
    {
      "token" : "国徽",
      "start_offset" : 7,
      "end_offset" : 9,
      "type" : "CN_WORD",
      "position" : 9
    }
  ]
}
```



## 映射

Elasticsearch 支持如下简单域类型：

- 字符串: `string`
- 整数 : `byte`, `short`, `integer`, `long`
- 浮点数: `float`, `double`
- 布尔型: `boolean`
- 日期: `date`

### 查看映射

### 更新映射

```shell
curl -X PUT "localhost:9200/article_post?pretty" -H 'Content-Type: application/json' -d'
{
  "mappings": {
    "article_post" : {
      "properties" : {
        "postid":{
        	"type" :    "integer",
        	"index":    "not_analyzed"
        },
        "title" : {
          "type" :    "text",
          "analyzer": "ik_max_word",
          "search_analyzer": "ik_smart"
        },
        "subtitle" : {
          "type" :    "text",
          "analyzer": "ik_max_word",
          "search_analyzer": "ik_smart"
        },
        "desc" : {
          "type" :    "text",
          "analyzer": "ik_max_word",
          "search_analyzer": "ik_smart"
        },
        "content" : {
          "type" :    "text",
          "analyzer": "ik_max_word",
          "search_analyzer": "ik_smart"
        },
        "mid" : {
          "type" :    "byte",
          "index":    "not_analyzed"
        },
        "platform" : {
          "type" :    "byte",
          "index":    "not_analyzed"
        },
        "versionContent" : {
          "type" :    "text",
          "analyzer": "ik_max_word",
          "search_analyzer": "ik_smart"
        }
      }
    }
  }
}
'
# 这么操作报错。elasticsearch7默认不在支持指定索引类型，默认索引类型是_doc，如果想改变，则配置include_type_name: true 即可
# 
{
  "error" : {
    "root_cause" : [
      {
        "type" : "mapper_parsing_exception",
        "reason" : "Root mapping definition has unsupported parameters:  [article_post : {properties={versionContent={search_analyzer=ik_smart, analyzer=ik_max_word, type=text}, subtitle={search_analyzer=ik_smart, analyzer=ik_max_word, type=text}, mid={index=not_analyzed, type=byte}, postid={index=not_analyzed, type=integer}, title={search_analyzer=ik_smart, analyzer=ik_max_word, type=text}, content={search_analyzer=ik_smart, analyzer=ik_max_word, type=text}, platform={index=not_analyzed, type=byte}, desc={search_analyzer=ik_smart, analyzer=ik_max_word, type=text}}}]"
      }
    ],
    "type" : "mapper_parsing_exception",
    "reason" : "Failed to parse mapping [_doc]: Root mapping definition has unsupported parameters:  [article_post : {properties={versionContent={search_analyzer=ik_smart, analyzer=ik_max_word, type=text}, subtitle={search_analyzer=ik_smart, analyzer=ik_max_word, type=text}, mid={index=not_analyzed, type=byte}, postid={index=not_analyzed, type=integer}, title={search_analyzer=ik_smart, analyzer=ik_max_word, type=text}, content={search_analyzer=ik_smart, analyzer=ik_max_word, type=text}, platform={index=not_analyzed, type=byte}, desc={search_analyzer=ik_smart, analyzer=ik_max_word, type=text}}}]",
    "caused_by" : {
      "type" : "mapper_parsing_exception",
      "reason" : "Root mapping definition has unsupported parameters:  [article_post : {properties={versionContent={search_analyzer=ik_smart, analyzer=ik_max_word, type=text}, subtitle={search_analyzer=ik_smart, analyzer=ik_max_word, type=text}, mid={index=not_analyzed, type=byte}, postid={index=not_analyzed, type=integer}, title={search_analyzer=ik_smart, analyzer=ik_max_word, type=text}, content={search_analyzer=ik_smart, analyzer=ik_max_word, type=text}, platform={index=not_analyzed, type=byte}, desc={search_analyzer=ik_smart, analyzer=ik_max_word, type=text}}}]"
    }
  },
  "status" : 400
}

# 参考官方文档 https://www.elastic.co/guide/en/elasticsearch/reference/current/removal-of-types.html

{
  "error" : {
    "root_cause" : [
      {
        "type" : "mapper_parsing_exception",
        "reason" : "Failed to parse mapping [_doc]: Failed to parse value [not_analyzed] as only [true] or [false] are allowed."
      }
    ],
    "type" : "mapper_parsing_exception",
    "reason" : "Failed to parse mapping [_doc]: Failed to parse value [not_analyzed] as only [true] or [false] are allowed.",
    "caused_by" : {
      "type" : "illegal_argument_exception",
      "reason" : "Failed to parse value [not_analyzed] as only [true] or [false] are allowed."
    }
  },
  "status" : 400
}

# 参考官网： https://www.elastic.co/guide/en/elasticsearch/reference/5.3/mapping.html

PUT /article_post
{
	"mappings": {
		"properties": {
			"postid": {
				"type": "integer"
			},
			"title": {
				"type": "text",
				"analyzer": "ik_max_word",
				"search_analyzer": "ik_smart"
			},
			"subtitle": {
				"type": "text",
				"analyzer": "ik_max_word",
				"search_analyzer": "ik_smart"
			},
			"desc": {
				"type": "text",
				"analyzer": "ik_max_word",
				"search_analyzer": "ik_smart"
			},
			"content": {
				"type": "text",
				"analyzer": "ik_max_word",
				"search_analyzer": "ik_smart"
			},
			"mid": {
				"type": "byte"
			},
			"platform": {
				"type": "byte"
			},
			"versionContent": {
				"type": "text",
				"analyzer": "ik_max_word",
				"search_analyzer": "ik_smart"
			}
		}
	}
}

创建成功提示：返回
{
  "acknowledged" : true,
  "shards_acknowledged" : true,
  "index" : "article_post"
}




```



### 测试映射



## 索引

查看所有索引列表

```shell
curl -XGET localhost:9200/_aliases?pretty
{
  ".monitoring-es-7-mb-2021.11.12" : {
    "aliases" : { }
  },
  ".monitoring-es-7-mb-2021.11.15" : {
    "aliases" : { }
  },
  ".monitoring-es-7-mb-2021.11.16" : {
    "aliases" : { }
  },
  "filebeat-7.15.0-2021.11.10-000002" : {
    "aliases" : {
      "filebeat-7.15.0" : {
        "is_write_index" : true
      }
    }
  },
  ".monitoring-es-7-mb-2021.11.13" : {
    "aliases" : { }
  },
  ".apm-custom-link" : {
    "aliases" : { }
  },
  ".apm-agent-configuration" : {
    "aliases" : { }
  },
  ".kibana_task_manager_7.14.1_001" : {
    "aliases" : {
      ".kibana_task_manager" : { },
      ".kibana_task_manager_7.14.1" : { }
    }
  },
  ".monitoring-es-7-mb-2021.11.10" : {
    "aliases" : { }
  },
  ".monitoring-es-7-mb-2021.11.11" : {
    "aliases" : { }
  },
  "filebeat-7.15.0-2021.10.11-000001" : {
    "aliases" : {
      "filebeat-7.15.0" : {
        "is_write_index" : false
      }
    }
  },
  ".kibana-event-log-7.14.1-000003" : {
    "aliases" : {
      ".kibana-event-log-7.14.1" : {
        "is_write_index" : true
      }
    }
  },
  ".async-search" : {
    "aliases" : { }
  },
  "metricbeat-7.14.1-2021.10.17-000002" : {
    "aliases" : {
      "metricbeat-7.14.1" : {
        "is_write_index" : true
      }
    }
  },
  "filebeat-7.14.1-2021.11.10-000003" : {
    "aliases" : {
      "filebeat-7.14.1" : {
        "is_write_index" : true
      }
    }
  },
  ".kibana-event-log-7.14.1-000001" : {
    "aliases" : {
      ".kibana-event-log-7.14.1" : {
        "is_write_index" : false
      }
    }
  },
  ".kibana-event-log-7.14.1-000002" : {
    "aliases" : {
      ".kibana-event-log-7.14.1" : {
        "is_write_index" : false
      }
    }
  },
  "filebeat-7.14.1-2021.09.11-000001" : {
    "aliases" : {
      "filebeat-7.14.1" : {
        "is_write_index" : false
      }
    }
  },
  ".monitoring-es-7-mb-2021.11.14" : {
    "aliases" : { }
  },
  "article_post" : {
    "aliases" : { }
  },
  "metricbeat-7.14.1-2021.09.17-000001" : {
    "aliases" : {
      "metricbeat-7.14.1" : {
        "is_write_index" : false
      }
    }
  },
  ".tasks" : {
    "aliases" : { }
  },
  ".kibana_7.14.1_001" : {
    "aliases" : {
      ".kibana" : { },
      ".kibana_7.14.1" : { }
    }
  },
  "filebeat-7.14.1-2021.10.11-000002" : {
    "aliases" : {
      "filebeat-7.14.1" : {
        "is_write_index" : false
      }
    }
  }
}
(base) [root@localhost elasticsearch]#
(base) [root@localhost elasticsearch]#
(base) [root@localhost elasticsearch]#
(base) [root@localhost elasticsearch]# curl -XGET localhost:9200/_aliases?pretty | grep post
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  2372  100  2372    0     0   566k      0 --:--:-- --:--:-- --:--:--  772k
  "article_post" : {
(base) [root@localhost elasticsearch]#
(base) [root@localhost elasticsearch]#
(base) [root@localhost elasticsearch]# curl -XGET localhost:9200/article_post?pretty
{
  "article_post" : {
    "aliases" : { },
    "mappings" : {
      "properties" : {
        "content" : {
          "type" : "text",
          "analyzer" : "ik_max_word",
          "search_analyzer" : "ik_smart"
        },
        "desc" : {
          "type" : "text",
          "analyzer" : "ik_max_word",
          "search_analyzer" : "ik_smart"
        },
        "mid" : {
          "type" : "byte"
        },
        "platform" : {
          "type" : "byte"
        },
        "postid" : {
          "type" : "integer"
        },
        "subtitle" : {
          "type" : "text",
          "analyzer" : "ik_max_word",
          "search_analyzer" : "ik_smart"
        },
        "title" : {
          "type" : "text",
          "analyzer" : "ik_max_word",
          "search_analyzer" : "ik_smart"
        },
        "versionContent" : {
          "type" : "text",
          "analyzer" : "ik_max_word",
          "search_analyzer" : "ik_smart"
        }
      }
    },
    "settings" : {
      "index" : {
        "routing" : {
          "allocation" : {
            "include" : {
              "_tier_preference" : "data_content"
            }
          }
        },
        "number_of_shards" : "1",
        "provided_name" : "article_post",
        "creation_date" : "1637038701776",
        "number_of_replicas" : "1",
        "uuid" : "HPLMERJTRUWKGQ9x6aPyOw",
        "version" : {
          "created" : "7150099"
        }
      }
    }
  }
}

```

向 article_post 索引写数据

```shell
curl -XPOST http://localhost:9200/article_post/_update/1 -H 'Content-Type:application/json' -d'
{
  "postid":1,
  "title":"原神",
  "subtitle:":"更新全新开放世界冒险游戏",
  "desc":"在七种元素交汇的大陆——「提瓦特」，每个人都可以成为神。 你从世界之外漂流而来，降临大地。在这广阔的世界中，你自由旅行、结识同伴、寻找掌控尘世元素的七神，直到与分离的血亲重聚，在终点见证一切旅途的沉淀。 维系者正在死去，创造者尚未到来。面对无法掌控的境遇，人类总会喟叹自身的无力…… 但在人生最陡峭的转折处，若有凡人的渴望达到极致，神明的视线就将投射而下。 当失散的双子在尘沙中重聚、世界的谜底在「神之眼」中尽数显现之时—— 旅行者，你将去往何方？ 《原神》是由米哈游自研的一款全新开放世界冒险RPG。你将在游戏中探索一个被称作「提瓦特」的幻想世界。 在这广阔的世界中，你可以踏遍七国，邂逅性格各异、能力独特的同伴，与他们一同对抗强敌，踏上寻回血亲之路；也可以不带目的地漫游，沉浸在充满生机的世界里，让好奇心驱使自己发掘各个角落的奥秘…… ",
  "content":"<p>在七种元素交汇的大陆——「提瓦特」，每个人都可以成为神。 你从世界之外漂流而来，降临大地。在这广阔的世界中，你自由旅行、结识同伴、寻找掌控尘世元素的七神，直到与分离的血亲重聚，在终点见证一切旅途的沉淀。 维系者正在死去，创造者尚未到来。面对无法掌控的境遇，人类总会喟叹自身的无力…… 但在人生最陡峭的转折处，若有凡人的渴望达到极致，神明的视线就将投射而下。 当失散的双子在尘沙中重聚、世界的谜底在「神之眼」中尽数显现之时—— 旅行者，你将去往何方？ 《原神》是由米哈游自研的一款全新开放世界冒险RPG。你将在游戏中探索一个被称作「提瓦特」的幻想世界。 在这广阔的世界中，你可以踏遍七国，邂逅性格各异、能力独特的同伴，与他们一同对抗强敌，踏上寻回血亲之路；也可以不带目的地漫游，沉浸在充满生机的世界里，让好奇心驱使自己发掘各个角落的奥秘…… </p><p><br></p>",
  "mid":0,
  "platform":1
}'
```

# 健康监测

## 查询健康状态

```shell
curl http://localhost:9200/_cluster/health
{
	"error": {
		"root_cause": [{
			"type": "security_exception",
			"reason": "missing authentication credentials for REST request [/_cluster/health]",
			"header": {
				"WWW-Authenticate": ["Basic realm=\"security\" charset=\"UTF-8\"", "ApiKey"] #需要验证
			}
		}],
		"type": "security_exception",
		"reason": "missing authentication credentials for REST request [/_cluster/health]",
		"header": {
			"WWW-Authenticate": ["Basic realm=\"security\" charset=\"UTF-8\"", "ApiKey"]
		}
	},
	"status": 401
}

[root@xinqiao docker-elk-main]#
[root@xinqiao docker-elk-main]#

[root@xinqiao docker-elk-main]# curl http://localhost:9200/_cluster/health -u elastic:changeme
{
	"cluster_name": "docker-cluster",
	"status": "yellow", # 状态
	"timed_out": false,
	"number_of_nodes": 1,
	"number_of_data_nodes": 1,
	"active_primary_shards": 16,
	"active_shards": 16,
	"relocating_shards": 0,
	"initializing_shards": 0,
	"unassigned_shards": 1,
	"delayed_unassigned_shards": 0,
	"number_of_pending_tasks": 0,
	"number_of_in_flight_fetch": 0,
	"task_max_waiting_in_queue_millis": 0,
	"active_shards_percent_as_number": 94.11764705882352
}

curl http://localhost:9200/_cluster/health?level=indices -u elastic:changeme

{
	"cluster_name": "docker-cluster",
	"status": "yellow",
	"timed_out": false,
	"number_of_nodes": 1,
	"number_of_data_nodes": 1,
	"active_primary_shards": 16,
	"active_shards": 16,
	"relocating_shards": 0,
	"initializing_shards": 0,
	"unassigned_shards": 1,
	"delayed_unassigned_shards": 0,
	"number_of_pending_tasks": 0,
	"number_of_in_flight_fetch": 0,
	"task_max_waiting_in_queue_millis": 0,
	"active_shards_percent_as_number": 94.11764705882352,
	"indices": {
		".ds-.logs-deprecation.elasticsearch-default-2022.03.31-000001": {
			"status": "green",
			"number_of_shards": 1,
			"number_of_replicas": 0,
			"active_primary_shards": 1,
			"active_shards": 1,
			"relocating_shards": 0,
			"initializing_shards": 0,
			"unassigned_shards": 0
		},
		".apm-agent-configuration": {
			"status": "green",
			"number_of_shards": 1,
			"number_of_replicas": 0,
			"active_primary_shards": 1,
			"active_shards": 1,
			"relocating_shards": 0,
			"initializing_shards": 0,
			"unassigned_shards": 0
		},
		".kibana_security_session_1": {
			"status": "green",
			"number_of_shards": 1,
			"number_of_replicas": 0,
			"active_primary_shards": 1,
			"active_shards": 1,
			"relocating_shards": 0,
			"initializing_shards": 0,
			"unassigned_shards": 0
		},
		".kibana_8.1.1_001": {
			"status": "green",
			"number_of_shards": 1,
			"number_of_replicas": 0,
			"active_primary_shards": 1,
			"active_shards": 1,
			"relocating_shards": 0,
			"initializing_shards": 0,
			"unassigned_shards": 0
		},
		".kibana-event-log-8.1.1-000001": {
			"status": "green",
			"number_of_shards": 1,
			"number_of_replicas": 0,
			"active_primary_shards": 1,
			"active_shards": 1,
			"relocating_shards": 0,
			"initializing_shards": 0,
			"unassigned_shards": 0
		},
		"kibana_sample_data_flights": {
			"status": "green",
			"number_of_shards": 1,
			"number_of_replicas": 0,
			"active_primary_shards": 1,
			"active_shards": 1,
			"relocating_shards": 0,
			"initializing_shards": 0,
			"unassigned_shards": 0
		},
		".tasks": {
			"status": "green",
			"number_of_shards": 1,
			"number_of_replicas": 0,
			"active_primary_shards": 1,
			"active_shards": 1,
			"relocating_shards": 0,
			"initializing_shards": 0,
			"unassigned_shards": 0
		},
		".geoip_databases": {
			"status": "green",
			"number_of_shards": 1,
			"number_of_replicas": 0,
			"active_primary_shards": 1,
			"active_shards": 1,
			"relocating_shards": 0,
			"initializing_shards": 0,
			"unassigned_shards": 0
		},
		".security-7": {
			"status": "green",
			"number_of_shards": 1,
			"number_of_replicas": 0,
			"active_primary_shards": 1,
			"active_shards": 1,
			"relocating_shards": 0,
			"initializing_shards": 0,
			"unassigned_shards": 0
		},
		"testindex": {
			"status": "yellow",# 找到哪个索引是 yellow
			"number_of_shards": 1,
			"number_of_replicas": 1,
			"active_primary_shards": 1,
			"active_shards": 1,
			"relocating_shards": 0,
			"initializing_shards": 0,
			"unassigned_shards": 1
		},
		".apm-custom-link": {
			"status": "green",
			"number_of_shards": 1,
			"number_of_replicas": 0,
			"active_primary_shards": 1,
			"active_shards": 1,
			"relocating_shards": 0,
			"initializing_shards": 0,
			"unassigned_shards": 0
		},
		"kibana_sample_data_ecommerce": {
			"status": "green",
			"number_of_shards": 1,
			"number_of_replicas": 0,
			"active_primary_shards": 1,
			"active_shards": 1,
			"relocating_shards": 0,
			"initializing_shards": 0,
			"unassigned_shards": 0
		},
		".kibana_task_manager_8.1.1_001": {
			"status": "green",
			"number_of_shards": 1,
			"number_of_replicas": 0,
			"active_primary_shards": 1,
			"active_shards": 1,
			"relocating_shards": 0,
			"initializing_shards": 0,
			"unassigned_shards": 0
		},
		"kibana_sample_data_logs": {
			"status": "green",
			"number_of_shards": 1,
			"number_of_replicas": 0,
			"active_primary_shards": 1,
			"active_shards": 1,
			"relocating_shards": 0,
			"initializing_shards": 0,
			"unassigned_shards": 0
		},
		".async-search": {
			"status": "green",
			"number_of_shards": 1,
			"number_of_replicas": 0,
			"active_primary_shards": 1,
			"active_shards": 1,
			"relocating_shards": 0,
			"initializing_shards": 0,
			"unassigned_shards": 0
		},
		".ds-ilm-history-5-2022.03.31-000001": {
			"status": "green",
			"number_of_shards": 1,
			"number_of_replicas": 0,
			"active_primary_shards": 1,
			"active_shards": 1,
			"relocating_shards": 0,
			"initializing_shards": 0,
			"unassigned_shards": 0
		}
	}
}
```



# 报刊库整个方案备忘

## 环境安装

1、Es版本和安装

```shell
cd /data/software
# 下载
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.2.0-x86_64.rpm
# rpm安装
rpm --install elasticsearch-8.2.0-x86_64.rpm

# 修改配置
vim /etc/elasticsearch/elasticsearch.yml

# ----------------------------------- Paths ------------------------------------
#
# Path to directory where to store the data (separate multiple locations by comma):
#
path.data: /data/www/server/esdata
#
# Path to log files:
#
path.logs: /data/www/server/eslog

切记 将 这两个目录的权限改一下
chown -R elasticsearch:elasticsearch ./esdata
chown -R elasticsearch:elasticsearch ./eslog

start automatically using systemd
 sudo systemctl daemon-reload
 sudo systemctl enable elasticsearch.service
### You can start elasticsearch service by executing
 sudo systemctl start elasticsearch.service
 

```



2、ik分词器版本和安装

```shell
cd /usr/share/elasticsearch/

./bin/elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v8.2.0/elasticsearch-analysis-ik-8.2.0.zip

重启
systemctl restart elasticsearch.service 生效
```

## ES配置

```shel
cd /etc/elasticsearch/jvm.options.d
vim jvm.options

-Xms4g
-Xmx4g
```

## index 映射

```shell
PUT /paper_articles

{
    "settings": {
        "index": {
          "sort.field": [ "pubdate", "created" ], 
          "sort.order": [ "desc", "desc" ]       
        }
      },
	"mappings": {
		"properties": {
			"id": {
				"type": "integer"
			},
			"newspaper_id": {
				"type": "integer"
			},
			"title": {
				"type": "text",
				"analyzer": "ik_max_word",
				"search_analyzer": "ik_smart"
			},
			"edition": {
				"type": "text",
				"analyzer": "ik_max_word",
				"search_analyzer": "ik_smart"
			},
			"position": {
				"type": "keyword",
				"index": false
			},
			"pubdate": {
				"type": "date"
			},
			"author": {
				"type": "keyword",
				"index": false
			},
			"url": {
				"type": "keyword",
				"index": false
			},
			"content": {
				"type": "text",
				"analyzer": "ik_max_word",
				"search_analyzer": "ik_smart"
			},
			"created": {
				"type": "integer"
			}
		}
	}
}




```



# [Elasticvue](chrome-extension://hkedbapjpblbodpgbajblpnlpenaebaa/index.html#/)

Chrome 插件 可以可视化管理es数据。


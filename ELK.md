# Kibana

```shell
https://www.elastic.co/guide/en/kibana/7.5/rpm.html#rpm-repo

①# cd /etc/yum.repos.d/

②# vim kibana.repo

[kibana-7.x]
name=Kibana repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md

③# yum install kibana


sudo /bin/systemctl daemon-reload
sudo /bin/systemctl enable kibana.service

④：启动服务
sudo systemctl start kibana.service
sudo systemctl stop kibana.service

systemctl restart kibana.service


Kibana rpm安装方式:
# wget https://artifacts.elastic.co/downloads/kibana/kibana-7.14.1-x86_64.rpm
# rpm --install kibana-7.14.1-x86_64.rpm

# systemctl start kibana.service
```

# ElasticSearch

```shell
https://www.elastic.co/guide/en/elasticsearch/reference/7.5/rpm.html#rpm-repo

①# cd /etc/yum.repos.d/

②# vim elasticsearch.repo

[elasticsearch]
name=Elasticsearch repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=0
autorefresh=1
type=rpm-md

③# yum install --enablerepo=elasticsearch elasticsearch

 sudo systemctl daemon-reload
 sudo systemctl enable elasticsearch.service
### You can start elasticsearch service by executing


④启动服务
# sudo systemctl start elasticsearch.service
Created elasticsearch keystore in /etc/elasticsearch


暂停服务
sudo systemctl stop elasticsearch.service


⑤ 验证是否成功
# curl http://127.0.0.1:9200
{
  "name" : "heibai",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "a6YdELdjTpizcyPcfdFTTQ",
  "version" : {
    "number" : "7.5.1",
    "build_flavor" : "default",
    "build_type" : "rpm",
    "build_hash" : "3ae9ac9a93c95bd0cdc054951cf95d88e1e18d96",
    "build_date" : "2019-12-16T22:57:37.835892Z",
    "build_snapshot" : false,
    "lucene_version" : "8.3.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}

```

# 搜索语法

```shell
#测试sort语法
GET article_post/_search
{
  "sort" : [
    {
      "postid": "desc" 
    }
  ],
  "query": {
    "match_all": {}
  }
}
```


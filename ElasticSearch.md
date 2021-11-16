# # 分词器

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


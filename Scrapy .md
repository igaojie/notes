# Scrapy 

## 安装指南

### 创建项目

```shell
(py3-7) # conda activate py3-7

(py3-7) # scrapy startproject bmsspider
New Scrapy project 'bmsspider', using template directory '/Users/xxx/opt/anaconda3/envs/py3-7/lib/python3.7/site-packages/scrapy/templates/project', created in:
    /Users/xxx/Works/bingmeishi/bmsspider

You can start your first spider with:
    cd bmsspider
    scrapy genspider example example.com
    

# cd bmsspider
    
# tree
.
├── bmsspider # 项目的python模块 代码会添加在这个下班
│   ├── __init__.py
│   ├── items.py # item文件
│   ├── middlewares.py # 中间件
│   ├── pipelines.py #pipelines文件
│   ├── settings.py #设置文件
│   └── spiders # 放置spider代码的目录
│       └── __init__.py
└── scrapy.cfg # 项目配置文件
    
    
```

### 创建爬虫

```shell
# scrapy genspider ruan8 ruan8.com
Created spider 'ruan8' using template 'basic' in module:
  bmsspider.spiders.ruan8
  
 tree
.
├── bmsspider
│   ├── __init__.py
│   ├── __pycache__
│   │   ├── __init__.cpython-36.pyc
│   │   ├── __init__.cpython-37.pyc
│   │   ├── settings.cpython-36.pyc
│   │   └── settings.cpython-37.pyc
│   ├── items.py
│   ├── middlewares.py
│   ├── pipelines.py
│   ├── settings.py
│   └── spiders
│       ├── __init__.py
│       ├── __pycache__
│       │   ├── __init__.cpython-36.pyc
│       │   ├── __init__.cpython-37.pyc
│       │   └── ruan8.cpython-37.pyc
│       └── ruan8.py # 新创建的爬虫的文件
└── scrapy.cfg
```

### 修改爬虫

```python
import scrapy


class Ruan8Spider(scrapy.Spider):
    name = 'ruan8' #用于区别Spider。 该名字必须是唯一的，您不可以为不同的Spider设定相同的名字。
    allowed_domains = ['ruan8.com']

    # 包含了Spider在启动时进行爬取的url列表。 因此，第一个被获取到的页面将是其中之一。 后续的URL则从初始的URL获取到的数据中提取。
    start_urls = [
        'https://www.ruan8.com/android/0_1.html', #安卓软件
        'https://www.ruan8.com/android/10_1.html', #安卓游戏
        'https://www.ruan8.com/iphone/0_1.html', # 苹果软件
        'https://www.ruan8.com/iphone/10_1.html' #苹果游戏
    ]

    #  被调用时，每个初始URL完成下载后生成的 Response 对象将会作为唯一的参数传递给该函数。 该方法负责解析返回的数据(response data)，提取数据(生成item)以及生成需要进一步处理的URL的 Request 对象。
    def parse(self, response):
        filename = response.url.split("/")[-2] +  "_" +response.url.split("/")[-1]
        
        with open(filename, 'wb') as f:
            f.write(response.body)
```

### 执行爬虫抓取

```shell
 scrapy crawl ruan8
2021-11-10 16:07:10 [scrapy.utils.log] INFO: Scrapy 2.4.1 started (bot: bmsspider)
2021-11-10 16:07:10 [scrapy.utils.log] INFO: Versions: lxml 4.6.3.0, libxml2 2.9.12, cssselect 1.1.0, parsel 1.5.2, w3lib 1.21.0, Twisted 21.2.0, Python 3.7.11 (default, Jul 27 2021, 07:03:16) - [Clang 10.0.0 ], pyOpenSSL 21.0.0 (OpenSSL 1.1.1l  24 Aug 2021), cryptography 35.0.0, Platform Darwin-21.1.0-x86_64-i386-64bit
2021-11-10 16:07:10 [scrapy.utils.log] DEBUG: Using reactor: twisted.internet.selectreactor.SelectReactor

# 配置项
2021-11-10 16:07:10 [scrapy.crawler] INFO: Overridden settings:
{'BOT_NAME': 'bmsspider',
 'NEWSPIDER_MODULE': 'bmsspider.spiders',
 'ROBOTSTXT_OBEY': True,
 'SPIDER_MODULES': ['bmsspider.spiders']}
2021-11-10 16:07:10 [scrapy.extensions.telnet] INFO: Telnet Password: 5510f46965f317ce

# 使用到的扩展
2021-11-10 16:07:10 [scrapy.middleware] INFO: Enabled extensions:
['scrapy.extensions.corestats.CoreStats',
 'scrapy.extensions.telnet.TelnetConsole',
 'scrapy.extensions.memusage.MemoryUsage',
 'scrapy.extensions.logstats.LogStats']
2021-11-10 16:07:10 [scrapy.middleware] INFO: Enabled downloader middlewares:
['scrapy.downloadermiddlewares.robotstxt.RobotsTxtMiddleware',
 'scrapy.downloadermiddlewares.httpauth.HttpAuthMiddleware',
 'scrapy.downloadermiddlewares.downloadtimeout.DownloadTimeoutMiddleware',
 'scrapy.downloadermiddlewares.defaultheaders.DefaultHeadersMiddleware',
 'scrapy.downloadermiddlewares.useragent.UserAgentMiddleware',
 'scrapy.downloadermiddlewares.retry.RetryMiddleware',
 'scrapy.downloadermiddlewares.redirect.MetaRefreshMiddleware',
 'scrapy.downloadermiddlewares.httpcompression.HttpCompressionMiddleware',
 'scrapy.downloadermiddlewares.redirect.RedirectMiddleware',
 'scrapy.downloadermiddlewares.cookies.CookiesMiddleware',
 'scrapy.downloadermiddlewares.httpproxy.HttpProxyMiddleware',
 'scrapy.downloadermiddlewares.stats.DownloaderStats']
2021-11-10 16:07:10 [scrapy.middleware] INFO: Enabled spider middlewares:
['scrapy.spidermiddlewares.httperror.HttpErrorMiddleware',
 'scrapy.spidermiddlewares.offsite.OffsiteMiddleware',
 'scrapy.spidermiddlewares.referer.RefererMiddleware',
 'scrapy.spidermiddlewares.urllength.UrlLengthMiddleware',
 'scrapy.spidermiddlewares.depth.DepthMiddleware']
2021-11-10 16:07:10 [scrapy.middleware] INFO: Enabled item pipelines:
[]
2021-11-10 16:07:10 [scrapy.core.engine] INFO: Spider opened
2021-11-10 16:07:10 [scrapy.extensions.logstats] INFO: Crawled 0 pages (at 0 pages/min), scraped 0 items (at 0 items/min)

# telnet 服务  127.0.0.1:6023
2021-11-10 16:07:10 [scrapy.extensions.telnet] INFO: Telnet console listening on 127.0.0.1:6023

# start_urls 列表内 逐个抓取 先看robots.txt 协议
2021-11-10 16:07:10 [scrapy.core.engine] DEBUG: Crawled (200) <GET https://www.ruan8.com/robots.txt> (referer: None)
2021-11-10 16:07:10 [scrapy.core.engine] DEBUG: Crawled (200) <GET https://www.ruan8.com/android/0_1.html> (referer: None)
2021-11-10 16:07:10 [scrapy.core.engine] DEBUG: Crawled (200) <GET https://www.ruan8.com/android/10_1.html> (referer: None)
2021-11-10 16:07:10 [scrapy.core.engine] DEBUG: Crawled (200) <GET https://www.ruan8.com/iphone/10_1.html> (referer: None)
2021-11-10 16:07:10 [scrapy.core.engine] DEBUG: Crawled (200) <GET https://www.ruan8.com/iphone/0_1.html> (referer: None)
2021-11-10 16:07:10 [scrapy.core.engine] INFO: Closing spider (finished)
2021-11-10 16:07:10 [scrapy.statscollectors] INFO: Dumping Scrapy stats:
{'downloader/request_bytes': 1139,
 'downloader/request_count': 5,
 'downloader/request_method_count/GET': 5,
 'downloader/response_bytes': 24276,
 'downloader/response_count': 5,
 'downloader/response_status_count/200': 5,
 'elapsed_time_seconds': 0.314606,
 'finish_reason': 'finished',
 'finish_time': datetime.datetime(2021, 11, 10, 8, 7, 10, 556859),
 'log_count/DEBUG': 5,
 'log_count/INFO': 10,
 'memusage/max': 51834880,
 'memusage/startup': 51834880,
 'response_received_count': 5,
 'robotstxt/request_count': 1,
 'robotstxt/response_count': 1,
 'robotstxt/response_status_count/200': 1,
 'scheduler/dequeued': 4,
 'scheduler/dequeued/memory': 4,
 'scheduler/enqueued': 4,
 'scheduler/enqueued/memory': 4,
 'start_time': datetime.datetime(2021, 11, 10, 8, 7, 10, 242253)}
2021-11-10 16:07:10 [scrapy.core.engine] INFO: Spider closed (finished)


 tree
.
├── android_0_1.html # 爬虫执行抓下来的网页文件
├── android_10_1.html # 爬虫执行抓下来的网页文件
├── bmsspider
│   ├── __init__.py
│   ├── __pycache__
│   │   ├── __init__.cpython-36.pyc
│   │   ├── __init__.cpython-37.pyc
│   │   ├── settings.cpython-36.pyc
│   │   └── settings.cpython-37.pyc
│   ├── items.py
│   ├── middlewares.py
│   ├── pipelines.py
│   ├── settings.py
│   └── spiders
│       ├── __init__.py
│       ├── __pycache__
│       │   ├── __init__.cpython-36.pyc
│       │   ├── __init__.cpython-37.pyc
│       │   └── ruan8.cpython-37.pyc
│       └── ruan8.py
├── iphone_0_1.html # 爬虫执行抓下来的网页文件
├── iphone_10_1.html # 爬虫执行抓下来的网页文件
└── scrapy.cfg
```



## 案例

### paopaoche.net

```python
# -*- coding: utf-8 -*-
import scrapy
import re
from config_spider.items import Item
from urllib.parse import urljoin, urlparse

def get_real_url(response, url):
    if re.search(r'^https?', url):
        return url
    elif re.search(r'^\/\/', url):
        u = urlparse(response.url)
        return u.scheme + ":" + url
    return urljoin(response.url, url)

class ConfigSpider(scrapy.Spider):
    name = 'config_spider'

    CLASS_ENUM = {
        '角色扮演': 4, 
        '动作冒险': 17, 
        '体育运动': 14, 
        '休闲益智': 12, 
        '模拟经营': 16, 
        '射击游戏': 13, 
        '策略塔防': 18, 
        '赛车竞速': 15, 
        '音乐舞蹈': 19, 
        '卡牌回合': 20, 
        '手机桌游': 21,
    }
    
    class_urls = ['https://www.paopaoche.net/android/jsby/', 'https://www.paopaoche.net/android/dzmx/', 'https://www.paopaoche.net/android/tyyd/', 'https://www.paopaoche.net/android/xxyz/', 'https://www.paopaoche.net/android/mnjy/', 'https://www.paopaoche.net/android/sjyx/', 'https://www.paopaoche.net/android/cltf/', 'https://www.paopaoche.net/android/scjs/', 'https://www.paopaoche.net/android/yywd/', 'https://www.paopaoche.net/android/kphh/', 'https://www.paopaoche.net/android/sjzy/']
    def start_requests(self):
        for url in self.class_urls:
        	yield scrapy.Request(url=url, callback=self.parse_list)

    def parse_list(self, response):
        prev_item = response.meta.get('item')
        for elem in response.xpath('//ul[@class="aznyList"]/li'):
            item = Item()
            item['title'] = elem.xpath('string(./a[@class="softname"]/@title)').extract_first()
            item['link'] = elem.xpath('string(./a[@class="softname"]/@href)').extract_first()
            item['cover'] = elem.xpath('string(.//img/@src)').extract_first()
            if prev_item is not None:
                for key, value in prev_item.items():
                    item[key] = value
            if not item['link']:
                continue
            yield scrapy.Request(url=get_real_url(response, item['link']), callback=self.parse_detail, meta={'item': item})
        next_url = response.xpath('//a[@class="next"]/@href').get()
        yield scrapy.Request(url=get_real_url(response, next_url), callback=self.parse_list, meta={'item': prev_item})

    def parse_detail(self, response):
        item = Item() if response.meta.get('item') is None else response.meta.get('item')
        item['version'] = response.xpath('//div[@class="right"]/p[1]/text()').extract_first()
        if item['version']:
            item['version'] = item['version'].strip()
            
        item['piclist'] = response.xpath('//div[@class="nyBannerimg"]/img/@src').getall()
        if item['piclist']:
            item['piclist'] = ",".join(list(map(lambda x: x.rstrip('\r').strip(), item['piclist'])))
        
        #item['content'] = response.xpath('string(//div[@id="game1"])').extract_first()
        item['size'] = response.xpath('//div[@class="infor"]/ul/li[contains(.,"大小")]/text()[2]').extract_first()
        if item['size']:
            item['size'] = item['size'].strip()
            
        item['classname'] = response.xpath('//div[@class="infor"]/ul/li[contains(.,"类别")]/text()[2]').extract_first()
        if item['classname']:
            item['classname'] = item['classname'].strip()
            item['classid'] = self.CLASS_ENUM.get(item['classname'], 0)
            
        
        #item['download'] = response.xpath('//li[@class="address_like"][1]/a/@href').extract_first()
        
        item['content'] = ''
        matchObj = re.search(
            r'<div class="azny_content" id="game1">(.*?)</div>', response.text, re.I | re.M | re.S)
        if matchObj:
            item['content'] = matchObj.group(1)
            
        yield item
```

### xiazaiba.com

```python
# -*- coding: utf-8 -*-
import scrapy
import re
from config_spider.items import Item
from urllib.parse import urljoin, urlparse

def get_real_url(response, url):
    if re.search(r'^https?', url):
        return url
    elif re.search(r'^\/\/', url):
        u = urlparse(response.url)
        return u.scheme + ":" + url
    return urljoin(response.url, url)

class ConfigSpider(scrapy.Spider):
    name = 'config_spider'
    
    CLASS_ENUM = {
        '生活服务': 23, 
        '社交通讯': 11, 
        '购物比价': 33, 
        '资讯阅读': 35, 
        '影音播放': 5 , 
        '图像拍照': 27, 
        '学习教育': 35, 
        '金融理财': 34, 
        '商务办公': 34, 
        '交通出行': 25, 
        '旅游住宿': 24, 
        '丽人母婴': 11, 
        '美食菜谱': 39, 
        '运动健身': 26, 
        '健康医疗': 30, 
        '主题美化': 28, 
        '安全防护': 28, 
        '系统软件': 28, 
        '趣味娱乐': 5, 
        '游戏工具': 28, 
        '休闲益智': 12, 
        '策略塔防': 18, 
        '动作冒险': 17, 
        '角色扮演': 4, 
        '飞行射击': 13, 
        '体育竞技': 14, 
        '赛车竞速': 15, 
        '棋牌卡牌': 20, 
        '养成经营': 16,
    }

    def start_requests(self):
        yield scrapy.Request(url='https://www.xiazaiba.com/android/app/index_1.html', callback=self.parse_list)
        yield scrapy.Request(url='https://www.xiazaiba.com/android/game/index_1.html', callback=self.parse_list)

    def parse_list(self, response):
        prev_item = response.meta.get('item')
        for elem in response.xpath('//ul[@class="down-list"]/li'):
            item = Item()
            item['title'] = elem.xpath('string(./div[@class="dlr fr"]/a/text())').extract_first()
            item['url'] = get_real_url(response, elem.xpath('./div[@class="dlr fr"]/a/@href').extract_first())
            item['cover'] = elem.xpath('string(./div[@class="dll fl"]/a/img/@src)').extract_first()
            item['desc'] = elem.xpath('string(./div[@class="dlr fr"]/div[@class="d3 mt10"]/text())').extract_first()
            if prev_item is not None:
                for key, value in prev_item.items():
                    item[key] = value
            if not item['url']:
                continue
            yield scrapy.Request(url=get_real_url(response, item['url']), callback=self.parse_detail, meta={'item': item})
        next_url = response.xpath('//div[@class="paginator"]/a[text()="下一页"]/@href').get()
        yield scrapy.Request(url=get_real_url(response, next_url), callback=self.parse_list, meta={'item': prev_item})

    def parse_detail(self, response):
        item = Item() if response.meta.get('item') is None else response.meta.get('item')
        version = u'版本：'
        item['version'] = response.xpath(f'string(//ul[@class="gc-2-con clearfix mt10"]/li[contains(., "{version}")]/text())').get()
        if item['version']:
            item['version'] = item['version'].replace(version, '')
            
        size = u'大小：'
        item['size'] = response.xpath(f'string(//ul[@class="gc-2-con clearfix mt10"]/li[contains(., "{size}")]/text())').get()
        if item['size']:
            item['size'] = item['size'].replace(size, '')
            
        classname = u'分类：'
        item['classname'] = response.xpath(f'string(//ul[@class="gc-2-con clearfix mt10"]/li[contains(., "{classname}")]/text())').get()
        if item['classname']:
            item['classname'] = item['classname'].replace(classname, '').strip()
            item['classid'] = self.CLASS_ENUM.get(item['classname'], 0)
            
            
        item['download'] = response.xpath('string(//ul[@class="gc-2-btn mt10"]/li/a/@href)').get()
        item['piclist'] = response.xpath('//li[@class="xtaber-item"]/img/@src').getall()
        if item['piclist']:
            item['piclist'] = ",".join(list(map(lambda x: x.rstrip('\r').strip(), item['piclist'])))
        #item['content'] = response.xpath('string(//div[@class="con add-con"])').get()
        item['content'] = ''
        matchObj = re.search(
            r'<div class="con add-con">(.*?)</div>', response.text, re.I | re.M | re.S)
        if matchObj:
            item['content'] = matchObj.group(1)
        yield item



```

### 海淀驾校班车

学以致用，查看海淀驾校的班车那几趟路过地铁站，挨个看太麻烦了。索性上代码。

```python
import string
import scrapy
from scrapy.utils.response import get_base_url 获取base_url 
from scrapy.utils.url import urljoin_rfc # 生成绝对连接地址
from bmsspider.items import HaijiaItem


class HaijiabusSpider(scrapy.Spider):
    name = 'haijiabus'
    #allowed_domains = ['http://www.haijia.com.cn/']
    domain = 'http://www.haijia.com.cn'
    start_urls = ['http://www.haijia.com.cn/line.html']

    def parse(self, response):
        for elem in response.xpath('//td[@class="pl_53"]'):
            item = HaijiaItem()
            item['line_name'] = elem.xpath('./a/text()').get()
            #print(response.url, elem.xpath('./a/@href').get())
            
            
            item['line_link'] = urljoin_rfc(get_base_url(response), elem.xpath('./a/@href').get() ).decode('utf-8')
            yield scrapy.Request(url = item['line_link'], callback=self.parse_detail, meta={'item': item})
            
        pass

    def parse_detail(self, response):
        item = response.meta['item'] if response.meta['item'] else HaijiaItem()
        line_stations= response.xpath('//table//tr/td[2]/text()').getall()
        #处理一下
        line_stations = list(map(lambda x: x.strip(), line_stations))
        item['line_stations'] = line_stations
        print(item)
        yield item




# 执行命令
scrapy crawl haijiabus -s ITEM_PIPELINES={} --loglevel=ERROR

 scrapy crawl --help                                                                
Usage
=====
  scrapy crawl [options] <spider>

Run a spider

Options
=======
--help, -h              show this help message and exit
-a NAME=VALUE           set spider argument (may be repeated)
--output=FILE, -o FILE  append scraped items to the end of FILE (use - for # 输出数据到某文件 -o haijiabus.json
                        stdout)
--overwrite-output=FILE, -O FILE
                        dump scraped items into FILE, overwriting any existing
                        file
--output-format=FORMAT, -t FORMAT
                        format to use for dumping items

Global Options
--------------
--logfile=FILE          log file. if omitted stderr will be used # 定义日志文件
--loglevel=LEVEL, -L LEVEL # 日志等级
                        log level (default: DEBUG)
--nolog                 disable logging completely
--profile=FILE          write python cProfile stats to FILE
--pidfile=FILE          write process ID to FILE
--set=NAME=VALUE, -s NAME=VALUE # 覆盖setting文件的设置参数
                        set/override setting (may be repeated)
--pdb                   enable pdb on failure
```



# Scrapyd

## 简介

Scrapyd 是一个运行 Scrapy 爬虫的服务程序，它提供一系列 HTTP 接口来帮助我们部署、启动、停止、删除爬虫程序。Scrapyd 支持版本管理，同时还可以管理多个爬虫任务，利用它我们可以非常方便地完成 Scrapy 爬虫项目的部署任务调度。

https://scrapyd.readthedocs.io/en/stable/

## 安装

pip install scrapyd

## 启动

```shell
# scrapyd

 scrapyd
2021-12-04T12:10:36+0800 [-] Loading /Users/xxx/opt/anaconda3/envs/py3-7/lib/python3.7/site-packages/scrapyd/txapp.py...
2021-12-04T12:10:36+0800 [-] Scrapyd web console available at http://127.0.0.1:6800/
2021-12-04T12:10:36+0800 [-] Loaded.
2021-12-04T12:10:36+0800 [twisted.scripts._twistd_unix.UnixAppLogger#info] twistd 21.2.0 (/Users/xxx/opt/anaconda3/envs/py3-7/bin/python 3.7.11) starting up.
2021-12-04T12:10:36+0800 [twisted.scripts._twistd_unix.UnixAppLogger#info] reactor class: twisted.internet.selectreactor.SelectReactor.
2021-12-04T12:10:36+0800 [-] Site starting on 6800
2021-12-04T12:10:36+0800 [twisted.web.server.Site#info] Starting factory <twisted.web.server.Site object at 0x7f8010b2d890>
2021-12-04T12:10:36+0800 [Launcher] Scrapyd 1.2.1 started: max_proc=64, runner='scrapyd.runner'

```

## 访问

http://127.0.0.1:6800/

```shell
curl http://localhost:6800/schedule.json -d project=default -d spider=somespider
{"node_name": "ShaodeMacBook-Pro.local", "status": "error", "message": "Scrapy 2.4.1 - no active project\n\nUnknown command: list\n\nUse \"scrapy\" to see available commands\n"}
(py3-7)  xxx@ShaodeMacBook-Pro  ~  
```





## 扩展

### scrapy-fake-useragent

参考：https://github.com/alecxe/scrapy-fake-useragent

```shell
pip install scrapy-fake-useragent
```

### Scrapy-Redis

![img](https://thsheep-wordpress.oss-cn-beijing.aliyuncs.com/62d1dc3038e8b596d6df6f8e8a28258a.jpg)



### *scrapy*-*redis*-bloomfilter

https://github.com/Python3WebSpider/ScrapyRedisBloomFilter

#### 安装

```shell
1. pip install scrapy-redis-bloomfilter

# 可以看到 依赖 redis-4.0.2 
"""
.......... 
Successfully installed deprecated-1.2.13 redis-4.0.2 scrapy-redis-0.7.1 scrapy-redis-bloomfilter-0.8.1 wrapt-1.13.3
安装完成
"""
```

#### 配置

```python
2. setting.py 追加

# Use this Scheduler, if your scrapy_redis version is <= 0.7.1
SCHEDULER = "scrapy_redis_bloomfilter.scheduler.Scheduler"

# Ensure all spiders share same duplicates filter through redis
DUPEFILTER_CLASS = "scrapy_redis_bloomfilter.dupefilter.RFPDupeFilter"

# Redis URL
REDIS_URL = 'redis://localhost:6379'

# Number of Hash Functions to use, defaults to 6
BLOOMFILTER_HASH_NUMBER = 6

# Redis Memory Bit of Bloom Filter Usage, 30 means 2^30 = 128MB, defaults to 30
BLOOMFILTER_BIT = 10

# Persist
SCHEDULER_PERSIST = True
```

#### 其他

```shell
(py3-7) [root@localhost full]# redis-cli
127.0.0.1:6379> keys *
1) "zol:requests"
2) "zol:bloomfilter"
127.0.0.1:6379> zcard zol:requests
(integer) 11123

# 该值为0的时候 爬取完毕。
127.0.0.1:6379> zcard zol:requests
(integer) 0
```

```shell
2021-12-02 11:10:49 [scrapy.core.engine] INFO: Closing spider (finished)
2021-12-02 11:10:49 [scrapy.extensions.feedexport] INFO: Stored json feed (16389 items) in: zol1202.json
2021-12-02 11:10:49 [scrapy.statscollectors] INFO: Dumping Scrapy stats:
{'bloomfilter/filtered': 42, #过滤条数
 'downloader/request_bytes': 6231191,
 'downloader/request_count': 17243,
 'downloader/request_method_count/GET': 17243,
 'downloader/response_bytes': 157482876,
 'downloader/response_count': 17243,
 
 # 下载返回状态码对应的数据条数
 'downloader/response_status_count/200': 17231, 
 'downloader/response_status_count/301': 1,
 'downloader/response_status_count/302': 1,
 'downloader/response_status_count/403': 9,
 'downloader/response_status_count/404': 1,
 
 'elapsed_time_seconds': 2417.790671,
 'finish_reason': 'finished',
 'finish_time': datetime.datetime(2021, 12, 2, 3, 10, 49, 87217),
 
 # 状态码异常数据忽略条数
 'httperror/response_ignored_count': 10,
 'httperror/response_ignored_status_count/403': 9,
 'httperror/response_ignored_status_count/404': 1,
 'item_scraped_count': 16389,
 'log_count/DEBUG': 35323,
 'log_count/INFO': 65,
 'memusage/max': 129052672,
 'memusage/startup': 70402048,
 'request_depth_max': 101,
 'response_received_count': 17241,
 'scheduler/dequeued/redis': 17243,
 'scheduler/enqueued/redis': 17243,
 'start_time': datetime.datetime(2021, 12, 2, 2, 30, 31, 296546)}
2021-12-02 11:10:49 [scrapy.core.engine] INFO: Spider closed (finished)
```



[海量数据处理算法Bloom Filter](https://www.bookstack.cn/read/piaosanlang-spiders/81823a91f0a28d5a.md)



## 命令行

### scrapy tools

#### fetch

```python
scrapy fetch --nolog --headers  https://www.wanyx.com/shoujigonglue/479039.html
> Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
> Accept-Language: en
> User-Agent: Scrapy/2.4.1 (+https://scrapy.org)
> Accept-Encoding: gzip, deflate
>
< Date: Wed, 10 Nov 2021 03:29:48 GMT
< Content-Type: text/html; charset=utf-8
< Expires: Wed, 10 Nov 2021 03:58:37 GMT
< Server: nginx
< Last-Modified: Wed, 10 Nov 2021 03:28:36 GMT
< Pragma: private
< Cache-Control: max-age=1800
< Age: 71
< X-Via: 1.1 PS-000-01grF34:4 (Cdn Cache Server V2.0), 1.1 PS-LFQ-01WbY32:6 (Cdn Cache Server V2.0)
< X-Ws-Request-Id: 618b3cac_PS-LFQ-012uT33_38043-18341
```

#### shell

```shell
scrapy shell https://www.ruan8.com/soft/65742.html

...

2021-11-11 12:11:34 [scrapy.core.engine] DEBUG: Crawled (200) <GET https://www.ruan8.com/soft/65742.html> (referer: None)
[s] Available Scrapy objects:
[s]   scrapy     scrapy module (contains scrapy.Request, scrapy.Selector, etc)
[s]   crawler    <scrapy.crawler.Crawler object at 0x7ff37cea4748>
[s]   item       {}
[s]   request    <GET https://www.ruan8.com/soft/65742.html>
[s]   response   <200 https://www.ruan8.com/soft/65742.html>
[s]   settings   <scrapy.settings.Settings object at 0x7ff37d33fac8>
[s]   spider     <Ruan8Spider 'ruan8' at 0x7ff37d6b7080>
[s] Useful shortcuts:
[s]   fetch(url[, redirect=True]) Fetch URL and update local objects (by default, redirects are followed)
[s]   fetch(req)                  Fetch a scrapy.Request and update local objects 
[s]   shelp()           Shell help (print this help)
[s]   view(response)    View response in a browser
>>> 

>>> esponse.css('div.game-info-c').extract_first()
>>> response.xpath('//div[@class="game-info-c"]/node()').extract()
```

## XPATH

### 文档

英文文档：https://developer.mozilla.org/en-US/docs/Web/XPath/Functions

中文文档：https://cloud.tencent.com/developer/chapter/17854 

### 语法

1. 层级： / 直接子级 //跳级
2. 属性:  @ eg. @href  @src
3. 函数 eg. position() contains() 

### 方法

#### position()

```python
>>> response.xpath('//ul[@class="catalog_nav"]/li[position()>1]/a/@href').getall()
['https://www.paopaoche.net/android/jsby/', 'https://www.paopaoche.net/android/dzmx/', 'https://www.paopaoche.net/android/tyyd/', 'https://www.paopaoche.net/android/xxyz/', 'https://www.paopaoche.net/android/mnjy/', 'https://www.paopaoche.net/android/sjyx/', 'https://www.paopaoche.net/android/cltf/', 'https://www.paopaoche.net/android/scjs/', 'https://www.paopaoche.net/android/yywd/', 'https://www.paopaoche.net/android/kphh/', 'https://www.paopaoche.net/android/sjzy/']

```



#### text()

```python
# 元素显示在页面上的文本

>>> response.xpath('//title')
[<Selector xpath='//title' data='<title>QQ音乐tv版下载_QQ音乐tv最新版下载_软吧下载</ti...'>]
>>> response.xpath('//title/text()')
[<Selector xpath='//title/text()' data='QQ音乐tv版下载_QQ音乐tv最新版下载_软吧下载'>]
>>> response.xpath('//title/text()').extract_first()
'QQ音乐tv版下载_QQ音乐tv最新版下载_软吧下载'

# 链接文本内容 = 下一页 的 写法
response.xpath('//div[@class="paginator"]/a[text()="下一页"]/@href').get()

```



#### contains()

https://www.zyte.com/blog/xpath-tips-from-the-web-scraping-trenches/

```python
sel.xpath("//a[contains(.//text(), 'Next Page')]").extract() # .get() #.getall()


# contains 
response.xpath(f'string(//ul[@class="gc-2-con clearfix mt10"]/li[contains(., "版本：")]/text())').get()
```



#### node()

#### 属性值获取

```python
# @src @loadsrc 支持 | 或的关系。
sel.xpath('./div[@class="summary-text"]/p/a/img/@src | ./div[@class="summary-text"]/p/a/img/@loadsrc').getall()

response.xpath('//a[@class="page-next"]/@href').get()
```

## CSS

## 正则表达式

### re.match

re.match 尝试从字符串的起始位置匹配一个模式，如果不是起始位置匹配成功的话，match()就返回none。

```python
re.match(pattern, string, flags=0)
flags :
  re.I # 大小写不区分
  re.L
  re.M # 多行匹配，影响 ^ 和 $
  re.S # 使 . 匹配包括换行在内的所有字符
  re.U
  re.X
```

```python

>>> str = "abc"
>>> m = re.match(r'.*(b).*', str)
>>> m.group()
'abc'
>>> m.group(1)
'b'

>>> m = re.match(r'.*(b).*', str, re.I|re.M|re.S)
>>> m.group()
'abc'
```

### re.search

```python
matchObj = re.search(r'<li>版本：(.*?)</li>', response.text, re.I|re.M|re.S)
if matchObj:
  item['version'] = matchObj.group(1)    

```



re.match只匹配字符串的开始，如果字符串开始不符合正则表达式，则匹配失败，函数返回None；

而re.search匹配整个字符串，直到找到一个匹配。



### re.complie





```scala
```

```mysql
```



## 调试

Chrome 或者 Firefox 中的调试方式

https://stackoverflow.com/questions/22571267/how-to-verify-an-xpath-expression-in-chrome-developers-tool-or-firefoxs-firebug/22571294#22571294

https://yizeng.me/2014/03/23/evaluate-and-validate-xpath-css-selectors-in-chrome-developer-tools/

![Evaluate XPath/CSS selectors using 'Elements' panel's seach function](https://yizeng.me/assets/images/posts/2014-03-23-evaluate-using-elements-panel.gif)

```console
$x('//title/text()')

$x('//div[@class="game-info-c"]')
```

```tex
1.打开 Chrome 浏览器 ，输入目标网址
2.快捷键 F12
3.点击 Elements
4.快捷键 Ctrl + F 
```

# 线程池

```python
import time
# 导入线程池模块对应的类
from multiprocessing.dummy import Pool

start_time = time.time()
def get_page(str):
    print("正在下载: ", str)
    time.sleep(2)
    print("下载成功： ", str)

name_list = ['aa','bb','cc','dd']

# 实例化一个线程池对象
pool = Pool(4)
# 将列表中每一个元素传递给 get_page 方法进行处理
pool.map(get_page, name_list)

end_time = time.time()

print(end_time - start_time)
```

# 协程

## 单线程

```pyhton
def request(url):
    print("正在请求的url 是：", url)
    print("请求成功.", url)

request("www.baidu.com")

 python test/async.py      
正在请求的url 是： www.baidu.com
请求成功. www.baidu.com
```

## 协程

Event_loop 使用

```pyt
async def request(url):
    print("正在请求的url 是：", url)
    print("请求成功.", url)
c = request("www.baidu.com")     # //sys:1: RuntimeWarning: coroutine 'request' was never awaited


# 创建一个事件循环对象
loop = asyncio.get_event_loop()
# 将协程对象注册到loop中，然后启动loop
loop.run_until_complete(c)
```

task 使用

```python
async def request(url):
    print("正在请求的url 是：", url)
    print("请求成功.", url)
c = request("www.baidu.com")     # //sys:1: RuntimeWarning: coroutine 'request' was never awaited

loop = asyncio.get_event_loop()
task = loop.create_task(c)
print(task)
loop.run_until_complete(task)
print(task)

# <Task pending coro=<request() running at test/async.py:11>>
# 正在请求的url 是： www.baidu.com
# 请求成功. www.baidu.com
# <Task finished coro=<request() done, defined at test/async.py:11> result=None>
```

feature 使用

```python
# feature 使用
loop = asyncio.get_event_loop()
task = asyncio.ensure_future(c)
print(task)
loop.run_until_complete(task)
print(task)


# <Task pending coro=<request() running at test/async.py:11>>
# 正在请求的url 是： www.baidu.com
# 请求成功. www.baidu.com
# <Task finished coro=<request() done, defined at test/async.py:11> result=None>
```

绑定回调

```python
import asyncio
async def request(url):
    print("正在请求的url 是：", url)
    print("请求成功.", url)

    return url
c = request("www.baidu.com")

def callback_func(task):
    # result 返回的就是任务对象中封装的协程对象对应函数的返回值
    print(task.result())

loop = asyncio.get_event_loop()
task = asyncio.ensure_future(c)
#将回调函数绑定到任务对象中
task.add_done_callback(callback_func)
loop.run_until_complete(task)

# 正在请求的url 是： www.baidu.com
# 请求成功. www.baidu.com
# www.baidu.com
```



## 多任务协程

```python
import asyncio
import time

async def request(url):
    print("正在下载...", url)

    # 在异步协程中，如果初选了同步模块相关的代码，那么就无法实现异步。
    #time.sleep(2)

    # 当在asyncio中遇到阻塞操作必须进行手动挂起 await关键字
    await asyncio.sleep(2)

    print("下载完毕....", url)


start = time.time()

urls = [
    "www.baidu.com",
    "www.sogou.com",
    "www.toutiao.com"
]


stasks = []
for url in urls:
    c = request(url)
    task = asyncio.ensure_future(c)
    stasks.append(task)

loop = asyncio.get_event_loop()
#将任务列表封装到wait 中 ，固定写法。不能将列表直接传给 run_until_complete 。

#loop.run_until_complete(stasks)
# Traceback (most recent call last):
#   File "test/multi_task.py", line 29, in <module>
#     loop.run_until_complete((stasks))
#   File "/Users/xxx/opt/anaconda3/envs/py3-7/lib/python3.7/asyncio/base_events.py", line 566, in run_until_complete
#     future = tasks.ensure_future(future, loop=self)
#   File "/Users/xxx/opt/anaconda3/envs/py3-7/lib/python3.7/asyncio/tasks.py", line 619, in ensure_future
#     raise TypeError('An asyncio.Future, a coroutine or an awaitable is '
# TypeError: An asyncio.Future, a coroutine or an awaitable is required

loop.run_until_complete(asyncio.wait(stasks))

print(time.time() - start)

```



# Crawlab 

Crawlab 分布式爬虫管理平台 https://docs.crawlab.cn/zh/QuickStart/

## docker 方式安装

1. 新建 docker-compose.yml 文件：

```yaml
version: '3.3'
services:
  master:
    image: tikazyq/crawlab:latest
    container_name: crawlab-master
    environment:
      SCRAPY_ENV: "PRO"
      CRAWLAB_SERVER_MASTER: "Y"
      CRAWLAB_MONGO_HOST: "mongo"
      CRAWLAB_REDIS_ADDRESS: "redis"
    ports:
      - "8080:8080" # frontend
      - "8000:8000" # backend
    sysctls:
      - net.ipv4.ip_forward=1
    depends_on:
      - mongo
      - redis
  worker01:
    image: tikazyq/crawlab:latest
    container_name: crawlab-worker-01
    environment:
      SCRAPY_ENV: "PRO"
      CRAWLAB_SERVER_MASTER: "N"
      CRAWLAB_MONGO_HOST: "mongo"
      CRAWLAB_REDIS_ADDRESS: "redis"
    depends_on:
      - mongo
      - redis
  worker02:
    image: tikazyq/crawlab:latest
    container_name: crawlab-worker-02
    environment:
      SCRAPY_ENV: "PRO"
      CRAWLAB_SERVER_MASTER: "N"
      CRAWLAB_MONGO_HOST: "mongo"
      CRAWLAB_REDIS_ADDRESS: "redis"
    depends_on:
      - mongo
      - redis
  mongo:
    image: mongo:4.4.10
    restart: always
    ports:
      - "27017:27017"
    volumes:
      - /data2/www/wwwroot/crawlab/mongo/db:/data/db
      - /data2/www/wwwroot/crawlab/mongo/log:/var/log/mongodb
      - /data2/www/wwwroot/crawlab/mongo/config:/etc/mongo
  redis:
    image: redis:latest
    restart: always
    ports:
      - "6379:6379"

```

2. docker-compose up -d 



# 斐波那契数列

## 简单实现

```python
# 直接在 fab 函数中用 print 打印数字会导致该函数可复用性较差，因为 fab 函数返回 None，其他函数无法获得该函数生成的数列。
def fab(max):
	n, a, b = 0, 0, 1
	while n < max:
		print(b)
		a, b =  b, a + b
		n = n + 1
    
fab(5)    
    
```

## List 实现

```python
# 该函数在运行中占用的内存会随着参数 max 的增大而增大，如果要控制内存占用，最好不要用 List
def fab(max):
    n, a, b = 0, 0, 1
    L = []
    while n < max:
        L.append(b)
        a, b = b, a + b 
        n = n +1
    return L    

for n in fab(5):
    print(n)  
```

## Iteration 实现

```python
# 利用 iterable 我们可以把 fab 函数改写为一个支持 iterable 的 class， Fab 类通过 next() 不断返回数列的下一个数，内存占用始终为常数，不过代码量大不简洁。
class Fab(object):

    def __init__(self, max):
        self.max = max
        self.n, self.a, self.b = 0, 0 , 1
        
    def __iter__(self):
        return self

    def __next__(self): # python 3版本需要实现 __next__方法
        if self.n  < self.max:
            r = self.b
            self.a, self.b = self.b, self.a + self.b
            self.n = self.n + 1
            return r
        raise StopIteration()  
        
        
for n in Fab(5):
    print(n)        
```

## yield 实现

```python
# 简洁性，同时又要获得 iterable 的效果,yield来实现。

def fab(max):
    n, a, b = 0, 0 , 1
    while n < max:
        yield b # 使用 yield

        a, b = b, a + b
        n = n + 1   

for n in fab(5):
    print(n)
    
```

# 工具

## curl2scrpay

https://michael-shub.github.io/curl2scrapy/

https://curlconverter.com/  Convert [curl](https://curl.se/docs/manual.html) commands to Python, JavaScript, PHP, R, Go, Rust, Elixir, Java, MATLAB, Ansible URI, Strest, Dart or JSON。



# 学习资料

1. https://python3webspider.cuiqingcai.com/ [《Python3网络爬虫开发实战》](https://python3webspider.cuiqingcai.com/) - 崔庆才
2. 






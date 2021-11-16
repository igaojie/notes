# Scrapy 

## 安装指南

### 创建项目

```shell
(py3-7) # conda activate py3-7

(py3-7) # scrapy startproject bmsspider
New Scrapy project 'bmsspider', using template directory '/Users/shaogaojie/opt/anaconda3/envs/py3-7/lib/python3.7/site-packages/scrapy/templates/project', created in:
    /Users/shaogaojie/Works/bingmeishi/bmsspider

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

### 方法

#### text()

```python
# 元素显示在页面上的文本

>>> response.xpath('//title')
[<Selector xpath='//title' data='<title>QQ音乐tv版下载_QQ音乐tv最新版下载_软吧下载</ti...'>]
>>> response.xpath('//title/text()')
[<Selector xpath='//title/text()' data='QQ音乐tv版下载_QQ音乐tv最新版下载_软吧下载'>]
>>> response.xpath('//title/text()').extract_first()
'QQ音乐tv版下载_QQ音乐tv最新版下载_软吧下载'


```



#### contains()

```python
sel.xpath("//a[contains(.//text(), 'Next Page')]").extract()

```



#### node()



## CSS

## 正则表达式



### re.match

re.match 尝试从字符串的起始位置匹配一个模式，如果不是起始位置匹配成功的话，match()就返回none。

```python
re.match(pattern, string, flags=0)
flags :
  re.I # 大小写不明白
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
#   File "/Users/shaogaojie/opt/anaconda3/envs/py3-7/lib/python3.7/asyncio/base_events.py", line 566, in run_until_complete
#     future = tasks.ensure_future(future, loop=self)
#   File "/Users/shaogaojie/opt/anaconda3/envs/py3-7/lib/python3.7/asyncio/tasks.py", line 619, in ensure_future
#     raise TypeError('An asyncio.Future, a coroutine or an awaitable is '
# TypeError: An asyncio.Future, a coroutine or an awaitable is required

loop.run_until_complete(asyncio.wait(stasks))

print(time.time() - start)

```





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


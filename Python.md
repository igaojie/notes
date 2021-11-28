# requirement.txt

1. 把环境中的依赖写入 requirement.txt 中 https://pip.pypa.io/en/stable/cli/pip_install/

```shell
> pip freeze > ./requirements.txt 

> cat ./requirements.txt

aiohttp==3.8.0
aiosignal==1.2.0
async-timeout==4.0.1
asynctest==0.13.0
attrs @ file:///tmp/build/80754af9/attrs_1620827162558/work   # Since version 19.1, pip also supports direct references
Automat @ file:///tmp/build/80754af9/automat_1600298431173/work
autopep8 @ file:///tmp/build/80754af9/autopep8_1620866417880/work
bcrypt @ file:///opt/concourse/worker/volumes/live/25966884-1a49-4448-62e7-44563eba29d2/volume/bcrypt_1597936171818/work
certifi==2021.10.8
cffi @ file:///opt/concourse/worker/volumes/live/4a8f7628-1917-402a-5a6a-cdafcc3ec547/volume/cffi_1625814707380/work
charset-normalizer==2.0.7
click==8.0.3
constantly==15.1.0
cryptography @ file:///opt/concourse/worker/volumes/live/3a00959b-c8b7-4878-6ff0-ab8e24d27201/volume/cryptography_1635366586923/work
cssselect==1.1.0
fake-useragent==0.1.11
Faker==9.8.2
Flask==2.0.2
frozenlist==1.2.0
hyperlink @ file:///tmp/build/80754af9/hyperlink_1610130746837/work
idna @ file:///tmp/build/80754af9/idna_1622654382723/work
importlib-metadata @ file:///opt/concourse/worker/volumes/live/f3c3a723-0733-424b-6c0e-af69ac95b59a/volume/importlib-metadata_1631916705096/work
incremental==17.5.0
iniconfig @ file:///home/linux1/recipes/ci/iniconfig_1610983019677/work
itemadapter @ file:///tmp/build/80754af9/itemadapter_1626442940632/work
itemloaders==1.0.4
itsdangerous==2.0.1
Jinja2==3.0.3
jmespath==0.10.0
lxml @ file:///opt/concourse/worker/volumes/live/a4879036-1466-48b6-6c8d-95f5cf7bc17d/volume/lxml_1616443217052/work
MarkupSafe==2.0.1
more-itertools @ file:///tmp/build/80754af9/more-itertools_1622818384463/work
multidict==5.2.0
packaging @ file:///tmp/build/80754af9/packaging_1625611678980/work
parsel @ file:///opt/concourse/worker/volumes/live/e81ee83a-e2f5-46f7-650e-db01191c338f/volume/parsel_1613041556142/work
pluggy @ file:///opt/concourse/worker/volumes/live/f85b5d5a-abad-4f46-46ad-4ead594efa77/volume/pluggy_1615976299968/work
Protego==0.1.16
py @ file:///tmp/build/80754af9/py_1607971587848/work
pyasn1 @ file:///Users/ktietz/demo/mc3/conda-bld/pyasn1_1629708007385/work
pyasn1-modules==0.2.8
pycodestyle @ file:///tmp/build/80754af9/pycodestyle_1615748559966/work
pycparser @ file:///tmp/build/80754af9/pycparser_1594388511720/work
PyDispatcher==2.0.5
PyHamcrest @ file:///tmp/build/80754af9/pyhamcrest_1615748656804/work
PyMySQL==1.0.2
pyOpenSSL @ file:///tmp/build/80754af9/pyopenssl_1635333100036/work
pyparsing @ file:///home/linux1/recipes/ci/pyparsing_1610983426697/work
pytest==6.2.4
pytest-runner @ file:///tmp/build/80754af9/pytest-runner_1623663836848/work
python-dateutil==2.8.2
queuelib==1.5.0
requests==2.26.0
Scrapy @ file:///opt/concourse/worker/volumes/live/21bd99f8-79fa-46a8-46c8-e8618719a350/volume/scrapy_1606864741462/work
scrapy-fake-useragent==1.4.4
scrapy-mysql-pipeline==2019.7.19
service-identity @ file:///Users/ktietz/demo/mc3/conda-bld/service_identity_1629460757137/work
six @ file:///tmp/build/80754af9/six_1623709665295/work
text-unidecode==1.3
toml @ file:///tmp/build/80754af9/toml_1616166611790/work
Twisted @ file:///opt/concourse/worker/volumes/live/3dd1a2f9-4715-4156-6acd-a4a3a6d6c798/volume/twisted_1614874390289/work
typing-extensions @ file:///tmp/build/80754af9/typing_extensions_1631814937681/work
urllib3==1.26.7
w3lib @ file:///Users/ktietz/demo/mc3/conda-bld/w3lib_1629359764703/work
Werkzeug==2.0.2
yarl==1.7.2
zipp @ file:///tmp/build/80754af9/zipp_1633618647012/work
zope.interface @ file:///opt/concourse/worker/volumes/live/e2e9977a-a3b7-4323-6e48-b61fb3ed115b/volume/zope.interface_1625036168072/work

```

2. 项目运行环境安装 requirement.txt 所包含的依赖

   ```shell
   pip install -r requirement.txt
   ```

   

3. xx

4. xx

5. ss

   

# Conda

https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html

## 创建环境

```shell
conda create -n py3-8 python=3.8
Collecting package metadata (current_repodata.json): done
Solving environment: done

## Package Plan ##

  environment location: /Users/shaogaojie/opt/anaconda3/envs/py3-8

  added / updated specs:
    - python=3.8


The following packages will be downloaded:

    package                    |            build
    ---------------------------|-----------------
    certifi-2021.10.8          |   py38hecd8cb5_0         151 KB
    ncurses-6.3                |       hca72f7f_2         856 KB
    pip-21.2.4                 |   py38hecd8cb5_0         1.8 MB
    python-3.8.12              |       h88f2d9e_0        10.3 MB
    setuptools-58.0.4          |   py38hecd8cb5_0         791 KB
    ------------------------------------------------------------
                                           Total:        13.9 MB

The following NEW packages will be INSTALLED:

  ca-certificates    pkgs/main/osx-64::ca-certificates-2021.10.26-hecd8cb5_2
  certifi            pkgs/main/osx-64::certifi-2021.10.8-py38hecd8cb5_0
  libcxx             pkgs/main/osx-64::libcxx-12.0.0-h2f01273_0
  libffi             pkgs/main/osx-64::libffi-3.3-hb1e8313_2
  ncurses            pkgs/main/osx-64::ncurses-6.3-hca72f7f_2
  openssl            pkgs/main/osx-64::openssl-1.1.1l-h9ed2024_0
  pip                pkgs/main/osx-64::pip-21.2.4-py38hecd8cb5_0
  python             pkgs/main/osx-64::python-3.8.12-h88f2d9e_0
  readline           pkgs/main/osx-64::readline-8.1-h9ed2024_0
  setuptools         pkgs/main/osx-64::setuptools-58.0.4-py38hecd8cb5_0
  sqlite             pkgs/main/osx-64::sqlite-3.36.0-hce871da_0
  tk                 pkgs/main/osx-64::tk-8.6.11-h7bc2e8c_0
  wheel              pkgs/main/noarch::wheel-0.37.0-pyhd3eb1b0_1
  xz                 pkgs/main/osx-64::xz-5.2.5-h1de35cc_0
  zlib               pkgs/main/osx-64::zlib-1.2.11-h1de35cc_3


Proceed ([y]/n)? y


Downloading and Extracting Packages
python-3.8.12        | 10.3 MB   | ################################################################################################ | 100%
ncurses-6.3          | 856 KB    | ################################################################################################ | 100%
pip-21.2.4           | 1.8 MB    | ################################################################################################ | 100%
certifi-2021.10.8    | 151 KB    | ################################################################################################ | 100%
setuptools-58.0.4    | 791 KB    | ################################################################################################ | 100%
Preparing transaction: done
Verifying transaction: done
Executing transaction: done
#
# To activate this environment, use
#
#     $ conda activate py3-8
#
# To deactivate an active environment, use
#
#     $ conda deactivate
```

## 激活环境

```shell
conda activate py3-8
```

# 进制转换

```python
# 进制转换
v1 = bin(25) # 十进制转成二进制
v2 = oct(25) # 十进制转成八进制
v3 = hex(25) # 十进制转成十六进制

print(v1, v2, v3) # 0b11001 0o31 0x19 字符串

v1 = int("0b11001", base=2) # 二进制转十进制
v2 = int("0o31", base=8) # 八进制转十进制
v3 = int("0x19", base=16) # 十六进制转十进制
print(v1, v2, v3) # 25 25 25

```



# 控制语句

## 三元运算符 *

python中没有其他语言中的三元表达式，不过有类似的实现方法。

```python
a = 1
b = 2
h = ""

h = "变量1" if a>b else "变量2"

print(h)

```



# 数据结构

## 字符串

% 格式化

```python
str_tpl = "大家好，我是%s,欢迎来到%s"
str = str_tpl % ("张三00", "我的世界")
print(str)

str_tpl = "大家好，我是%(name)s,欢迎来到%(place)s"

str = str_tpl % {"name":"张三01","place":"我的世界"}
print(str)

# 用两个百分号 输出 %
str_tpl = "大家好，我是%s, 今天的课程已经讲了90%%了"
str = str_tpl % ("张三04")
print(str)
```

format 格式化（推荐）

```python
str_tpl = "大家好，我是{0},欢迎来到{1}"

str = str_tpl .format("张三02", "我的世界")
print(str)

str_tpl = "大家好，我是{name},欢迎来到{place}"
str = str_tpl .format(name = "张三03", place = "我的世界")
print(str)
```

f 格式化(Python 3.6开始)

```python
name = "张三"
place = "我的世界"
str = f"大家好，我是{name},欢迎来到{place}" # 大家好，我是张三,欢迎来到我的世界
print(str)

str = f"大家好，我是{name},欢迎来到{place},我的等级是{9+2}" # 大家好，我是张三,欢迎来到我的世界,我的等级是11
print(str)

# 3.8 开始
str = f"大家好，我是{name},欢迎来到{place},我的等级是{9+2=}" # 大家好，我是张三,欢迎来到我的世界,我的等级是9+2=11
print(str)
# 3.7 版本会报错。
#  File "<fstring>", line 1
#    (9+2=)
#        ^
#SyntaxError: invalid syntax

str = f"我今年{22}岁"
print(str)

str = f"我今年{22:#b}岁"
print(str)

str = f"我今年{22:#o}岁"
print(str)

str = f"我今年{22:#x}岁"
print(str)

name = "sophia"
str = f"我叫{name.upper()}"
print(str)
```



## List 

### append

```python
aList = [123, 'xyz', 'zara', 'abc']; # 声明一个列表
aList.append( 2009 ); # 追加一个元素
```

### join

```python
sentence = ['this', 'is', 'a', 'sentence']
'-'.join(sentence) # 拼接成字符串
```

### del



```python

```

## Dict

### del

删除字典元素，能删单一的元素也能清空字典，清空只需一项操作。

```python


dict = {'Name': 'Zara', 'Age': 7, 'Class': 'First'}
dict.clear() # 清空字典所有条目

del item['Name'] # 删除 键 为 Name 的条目

del item # 删除字典
```







## string

### replace

```python
str.replace(old, new[, max]) 语法
old -- 将被替换的子字符串。
new -- 新字符串，用于替换old子字符串。
max -- 可选字符串, 替换不超过 max 次


>>> str = "this is string example....wow!!! this is really string"
>>> print(str.replace("is", "was")) # 将is 替换成was ，全部，遇到就替换
thwas was string example....wow!!! thwas was really string
>>> print(str.replace("is", "was",1)) # 第三个参数 可选, 替换不超过 max 次
thwas is string example....wow!!! this is really string
```

### format

基本语法是通过 **{}** 和 **:** 来代替以前的 **%** 。

```python
a = ['a1', 'a2', 'a3','a4']
b = ['b1', 'b2', 'b3']
e = ('c1')
f = ('d1', 'd2', 'd3')

print("{} {}".format("a", "b")) # a b
print("{} {}".format(a, b))  # ['a1', 'a2', 'a3', 'a4'] ['b1', 'b2', 'b3']
print("{1} {0}".format(a, b))  # ['b1', 'b2', 'b3'] ['a1', 'a2', 'a3', 'a4'] 
print("{}" . format(e)) # c1
print("{}". format(f)) # ('d1', 'd2', 'd3')
```



## 元组

Python的元组与列表类似，不同之处在于元组的元素不能修改。

元组使用小括号，列表使用方括号。

### tuple

```python
>>>tuple([1,2,3,4])
 
(1, 2, 3, 4)
 
>>> tuple({1:2,3:4})    #针对字典 会返回字典的key组成的tuple
 
(1, 3)
 
>>> tuple((1,2,3,4))    #元组会返回元组自身
 
(1, 2, 3, 4)
```





# lambda

不需要显式地定义函数，直接传入匿名函数更方便。

```python
>>> list(map(lambda x: x * x, [1, 2, 3, 4, 5, 6, 7, 8, 9]))
[1, 4, 9, 16, 25, 36, 49, 64, 81]

# lambda x: x * x 相当于下面：
def f(x):
    return x * x

```

匿名函数有个限制，就是只能有一个表达式，不用写`return`，返回值就是该表达式的结果。

# 函数

## *args,**kwargs

这两个是python中的可变参数。*args表示任何多个无名参数，它是一个tuple；**kwargs表示关键字参数，它是一个dict。并且同时使用*args和kwargs时，必须*args参数列要在**kwargs前，像foo(a=1, b='2', c=3, a', 1, None, )这样调用的话，会提示语法错误“SyntaxError: non-keyword arg after keyword arg”。

```python
def foo(*args, **kwargs):
    print("args = ", args)
    print("kwargs = ", kwargs)

    print("-" * 50)

#if __name__ == "__name__":
    
foo()
foo(a=1, b=2, c=3)
foo(1, 2, 3,4, a=1, b=2, c=3)
foo('a', 1, None, a =1, b = '2', c=3)

l = [1, 2, 3]
print(*l) # 1, 2, 3
```





# 生成器

### yield

```python
def my_generator(*args):
    print("starting up")
    yield 1

    print("work in ")
    yield 2

    print("still work in ")
    yield 3

    print("done")
    yield 4

# for n in my_generator():
#     print("-----",n)

gen = my_generator()

while True:
    try:
        n = gen.__next__()
    except StopIteration:
        break
    else:
        print("------", n)    
        
        
output::
starting up
------ 1
work in 
------ 2
still work in 
------ 3
done
------ 4        
```



# 内置函数

## zip

**zip()** 函数用于将可迭代的对象作为参数，将对象中对应的元素打包成一个个元组，然后返回由这些元组组成的列表。

如果各个迭代器的元素个数不一致，则返回列表长度与最短的对象相同，利用 * 号操作符，可以将元组解压为列表。

```python
a = ['a1', 'a2', 'a3','a4']
b = ['b1', 'b2', 'b3']

zipped = zip(a, b)

print(zipped) # 返回对象 <zip object at 0x7fef16e29cd0>
print(list(zipped))  # 转换为列表 [('a1', 'b1'), ('a2', 'b2'), ('a3', 'b3')]  元素个数与最短的列表一致

aa, bb = zip(*zip(a, b))
print(list(aa)) # ['a1', 'a2', 'a3'] 与 zip 相反，zip(*) 可理解为解压，返回二维矩阵式

c = ', '.join('%s=%s' % t for t in zip(a, b))
print(c) # a1=b1, a2=b2, a3=b3

d = ', '.join('{}={}'.format(*t) for t in zip(a, b))
print(d) ## a1=b1, a2=b2, a3=b3

```

https://stackoverflow.com/questions/7277072/smartest-way-to-join-two-lists-into-a-formatted-string

```python
from timeit import Timer
from itertools import izip

n = 300

a = [str(f) for f in range(n)]
b = [str(f) for f in range(n)]

def func1():
    return ', '.join([aa+'='+bb for aa in a for bb in b if a.index(aa) == b.index(bb)])

def func2():
    list = []
    for i in range(len(a)):
        list.append('%s=%s' % (a[i], b[i]))
    return ', '.join(list)

def func3():
    return ', '.join('%s=%s' % t for t in zip(a, b))

def func4():
    return ', '.join('%s=%s' % t for t in izip(a, b)) # itertools 提供的izip 方法

def func5():
    pat = n * '%s=%%s, '
    return pat % tuple(a) % tuple(b)

d = dict(zip((1,2,3,4,5),('heavy','append','zip','izip','% formatting')))
for i in xrange(1,6):
    t = Timer(setup='from __main__ import func%d'%i, stmt='func%d()'%i)
    print 'func%d = %s  %s' % (i,t.timeit(10),d[i])
    
    
    
func1 = 16.2272833558  heavy
func2 = 0.00410247671143  append
func3 = 0.00349569568199  zip
func4 = 0.00301686387516  izip
func5 = 0.00157338432678  % formatting    
```

# 常用标准库

## itertools

itertools - 为高效循环而创建迭代器的函数.



# 框架

## twisted

## Flask

### 安装

```python
pip install -U Flask
```

```python
from flask import Flask
import time

app = Flask(__name__)

@app.route('/a')
def index_a():
    time.sleep(2)
    return 'hello a'

@app.route('/b')
def index_b():
    time.sleep(2)
    return 'hello b'

@app.route('/c')
def index_c():
    time.sleep(2)
    return 'hello c'

if __name__ == '__main__':
    app.run(threaded=True)            
    
    

```

```shell
python test/flask_server.py
 * Serving Flask app 'flask_server' (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
127.0.0.1 - - [14/Nov/2021 09:52:17] "GET / HTTP/1.1" 404 -
127.0.0.1 - - [14/Nov/2021 09:52:17] "GET /favicon.ico HTTP/1.1" 404 -
127.0.0.1 - - [14/Nov/2021 09:52:18] "GET / HTTP/1.1" 404 -
127.0.0.1 - - [14/Nov/2021 09:52:23] "GET /a HTTP/1.1" 200 -
127.0.0.1 - - [14/Nov/2021 09:52:28] "GET /b HTTP/1.1" 200 -
127.0.0.1 - - [14/Nov/2021 09:52:32] "GET /c HTTP/1.1" 200 -
127.0.0.1 - - [14/Nov/2021 09:57:43] "GET /a HTTP/1.1" 200 -
127.0.0.1 - - [14/Nov/2021 09:57:45] "GET /b HTTP/1.1" 200 -
127.0.0.1 - - [14/Nov/2021 09:57:47] "GET /c HTTP/1.1" 200 -
```

# 常用包

## execjs

## HTTPConnection






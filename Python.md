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






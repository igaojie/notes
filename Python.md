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

# 框架

## twisted

# Flask

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




# GoLang

## 安装

### centos 安装

1. [下载](https://go.dev/doc/install)

   ```shell
   cd /data2/www/server
   
   wget https://dl.google.com/go/go1.17.4.linux-amd64.tar.gz
   ```

   

2. 安装

   ```shell
   # 删掉旧版本 解压新版本到 /usr/local 目录
   rm -rf /usr/local/go && tar -C /usr/local -xzf go1.17.4.linux-amd64.tar.gz
   
   # 环境变量
   export PATH=$PATH:/usr/local/go/bin
   ```

3. 确认

   ```shell
   go version
   go version go1.17.4 linux/amd64
   
   ```

   

## 基础

### 基础语法

1. 声明变量

   ```go
   //var a string = "Hello"
   	//关键字 变量名 变量类型 = 变量值
   	b := "Hello" //隐式声明变量
   ```

   

2. 关键字

3. 包

   1、首字母大小写区分 公有 私有

   2、包的引入 别名 . 

4. 基本数据类型

   1. 整数类型

   2. 浮点类型

   3. 字符串类型

   4. 布尔类型

   5. 判断数据类型的方法

      ```go
      var b int8  //0-255
      	var c uint8 //-128 - 127
      
      	b = 127
      	c = 255
      	fmt.Println(b)
      	fmt.Println(c)
      
      	fmt.Printf("%T", c)
      ```

      

   6. 数据类型转换

      ```go
      string1 := "字符串"
      	num1 := 123
      	num2 := 123.123
      	bool1 := true
      
      	fmt.Printf("%T,%T,%T,%T", string1, num1, num2, bool1)
      
      //string,int,float64,bool
      
      
      ```

5. 复杂数据类型

   1. 结构体

      ```go
      package main
      
      import "fmt"
      
      type user struct {
      	Name    string
      	Age     int
      	Sex     bool
      	Hobbies []string
      }
      
      func main() {
      	var u user
      	u.Age = 18
      	u.Name = "a"
      	u.Sex = true
      	u.Hobbies = []string{"喝酒", "唱歌", "烫头"}
      	fmt.Println(u)
      
      	u1 := user{
      		"b", 22, false, []string{"写代码"},
      	}
      	fmt.Println(u1)
      
      	u2 := new(user)
      	fmt.Println(u2)
      
      	u3 := user{
      		Name:    "u3",
      		Age:     99,
      		Sex:     false,
      		Hobbies: []string{"吃火锅"},
      	}
      	fmt.Println(u3)
      }
      
      func m() func(num int) {
      	return func(num int) {
      		fmt.Println(num + num)
      	}
      }
      
      ```

      

   2. 接口

   3. 数组

      1. 声明

          ```go
          a := [3]int{0, 1, 2}
          	fmt.Println(a)
          
          	b := [...]int{1, 2, 34, 34, 9, 43, 4,}
          	fmt.Println(b)
          
          	var c = new([10]int)
          	c[5] = 3
          	fmt.Println(c)
          ```

         

      2. 循环

         ```go
         z := [...]string{"a", "b", "c"}
         
         	for i := 0; i < len(z); i++ {
         		fmt.Println(z[i] + " ")
         	}
         
         	for i, v := range z {
         		fmt.Println(i, v)
         	}
         ```

         

      3. ss

      4. xx

   4. 切片

      ```go
      s := make([]int, 4, 6)
      	fmt.Println(s)
      
      	s = append(s, 1)
      	fmt.Println(s)
      
      	var s1 []int
      	fmt.Println(s1)
      ```

      

   5. map

      1. 声明

         ```go
         var m map[string]string
         	m = map[string]string{}
         	m["name"] = "张三"
         	m["sex"] = "性别"
         	fmt.Println(m)
         
         	m1 := map[string]string{}
         	m1["name"] = "张三"
         	m1["sex"] = "性别"
         	fmt.Println(m1)
         
         	m2 := make(map[string]string)
         	m2["name"] = "张三"
         	m2["sex"] = "性别"
         	fmt.Println(m2)
         
         m3 := map[int]interface{}{}
         	m3[1] = true
         	m3[2] = "A"
         	m3[3] = 1
         	fmt.Println(m3)
         ```

         

      2. 2

      3. 3

      4. 

   6. 指针

   7. 函数

      ```go
      func main() { 
      	(func() {
      		fmt.Println("AAA")
      	})()
      
      	m()(4)
      
      	f := func() {
      		fmt.Println("f")
      	}
      	f()
      }
      
      func m() func(num int) {
      	return func(num int) {
      		fmt.Println(num + num)
      	}
      }   
      ```

      

   8. 管道

6. 流程控制语句

   1. if

   2. switch case

      ```go
      a := 1
      	switch a {
      	case 0:
      		fmt.Println("0")
      	case 1:
      		fmt.Println("1")
      	default:
      		fmt.Println("d")
      	}
      ```

      

   3. for

      ```go
      for {
      		a++
      		fmt.Println(a)
      		if a >= 9 {
      			break
      		}
      	}
      
      	for b := 0; b < 10; b++ {
      		fmt.Println(b)
      	}
      
      	c := 0
      	for c < 10 {
      		c++
      		fmt.Println(c)
      	}
      ```

      

   4. goto

      ```go
      a := 0
      
      A:
      	for a < 10 {
      		a++
      
      		if a == 5 {
      			break A
      			goto B
      		}
      	}
      B:
      	fmt.Println("BBBBB")
      }
      ```

      

## 包

### sync



## 框架

### Beego

#### 安装

````shell
cd /data2/www/wwwroot/go
go get github.com/beego/beego/v2@v2.0.0

```
go get: github.com/beego/beego/v2@v2.0.0: Get "https://proxy.golang.org/github.com/beego/beego/v2/@v/v2.0.0.info": dial tcp 142.251.42.241:443: i/o timeout
```
````

 "https://proxy.golang.org/github.com/beego/beego/v2/@v/v2.0.0.info": dial tcp 142.251.42.241:443: i/o timeout 的问题解决方案

```shell
[root@localhost go]# go env
GO111MODULE="" # 开启
GOARCH="amd64"
GOBIN=""
GOCACHE="/root/.cache/go-build"
GOENV="/root/.config/go/env"
GOEXE=""
GOEXPERIMENT=""
GOFLAGS=""
GOHOSTARCH="amd64"
GOHOSTOS="linux"
GOINSECURE=""
GOMODCACHE="/root/go/pkg/mod"
GONOPROXY=""
GONOSUMDB=""
GOOS="linux"
GOPATH="/root/go"
GOPRIVATE=""
GOPROXY="https://proxy.golang.org,direct" # 使用 goproxy.cn 
GOROOT="/usr/local/go"
GOSUMDB="sum.golang.org"
GOTMPDIR=""
GOTOOLDIR="/usr/local/go/pkg/tool/linux_amd64"
GOVCS=""
GOVERSION="go1.17.4"
GCCGO="gccgo"
AR="ar"
CC="gcc"
CXX="g++"
CGO_ENABLED="1"
GOMOD="/dev/null"
CGO_CFLAGS="-g -O2"
CGO_CPPFLAGS=""
CGO_CXXFLAGS="-g -O2"
CGO_FFLAGS="-g -O2"
CGO_LDFLAGS="-g -O2"
PKG_CONFIG="pkg-config"
GOGCCFLAGS="-fPIC -m64 -pthread -fmessage-length=0 -fdebug-prefix-map=/tmp/go-build747443546=/tmp/go-build -gno-record-gcc-switches"


# 执行一下命令：
[root@localhost go]# go env -w GO111MODULE=on
[root@localhost go]# go env -w GOPROXY=https://goproxy.cn,direct
[root@localhost go]#

# 再次执行安装beego
# go get github.com/beego/beego/v2@v2.0.0
go: downloading github.com/beego/beego/v2 v2.0.0

```






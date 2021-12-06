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






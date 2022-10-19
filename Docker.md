# 架构

![img](https://www.runoob.com/wp-content/uploads/2016/04/576507-docker1.png)



# 安装

## Centos 8 安装 Docker

https://docs.docker.com/engine/install/centos/

0. Centos 版本

   ```shell
   # uname -r
   4.18.0-348.2.1.el8_5.x86_64
   
   # cat /etc/redhat-release
   CentOS Linux release 8.5.2111
   ```

   

1. 卸载已安装docker ，若没有，忽略。

   ```shell
   yum remove docker \
                     docker-client \
                     docker-client-latest \
                     docker-common \
                     docker-latest \
                     docker-latest-logrotate \
                     docker-logrotate \
                     docker-engine
   ```

   

2. 设置仓库

   ```shell
   yum install -y yum-utils
   yum-config-manager \
       --add-repo \
       https://download.docker.com/linux/centos/docker-ce.repo
   ```

   

3. 安装docker 引擎

   ```shell
   > yum install docker-ce docker-ce-cli containerd.io
   
   
   # docker 与 podman 冲突。 直接卸载掉 podman
   > yum remove podman buildah 。
   次元数据过期检查：0:04:18 前，执行于 2021年11月24日 星期三 18时05分23秒。
   错误：
    问题 1: 安装的软件包的问题 podman-3.3.1-9.module_el8.5.0+988+b1f0b741.x86_64
     - 软件包 podman-3.3.1-9.module_el8.5.0+988+b1f0b741.x86_64 需要 runc >= 1.0.0-57，但没有提供者可以被安装
     - 软件包 containerd.io-1.4.12-3.1.el8.x86_64 与 runc（由 runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64 提供）冲突
     - 软件包 containerd.io-1.4.12-3.1.el8.x86_64 取代了 runc（由 runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64 提供）
     - 无法为该任务安装最佳候选
     - 软件包 runc-1.0.0-66.rc10.module_el8.5.0+1004+c00a74f5.x86_64 被模块过滤过滤掉
     - 软件包 runc-1.0.0-72.rc92.module_el8.5.0+1006+8d0e68a2.x86_64 被模块过滤过滤掉
    问题 2: 安装的软件包的问题 buildah-1.22.3-2.module_el8.5.0+911+f19012f9.x86_64
     - 软件包 buildah-1.22.3-2.module_el8.5.0+911+f19012f9.x86_64 需要 runc >= 1.0.0-26，但没有提供者可以被安装
     - 软件包 docker-ce-3:20.10.11-3.el8.x86_64 需要 containerd.io >= 1.4.1，但没有提供者可以被安装
     - 软件包 containerd.io-1.4.10-3.1.el8.x86_64 与 runc（由 runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64 提供）冲突
     - 软件包 containerd.io-1.4.10-3.1.el8.x86_64 取代了 runc（由 runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64 提供）
     - 软件包 containerd.io-1.4.11-3.1.el8.x86_64 与 runc（由 runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64 提供）冲突
     - 软件包 containerd.io-1.4.11-3.1.el8.x86_64 取代了 runc（由 runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64 提供）
     - 软件包 containerd.io-1.4.12-3.1.el8.x86_64 与 runc（由 runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64 提供）冲突
     - 软件包 containerd.io-1.4.12-3.1.el8.x86_64 取代了 runc（由 runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64 提供）
     - 软件包 containerd.io-1.4.3-3.1.el8.x86_64 与 runc（由 runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64 提供）冲突
     - 软件包 containerd.io-1.4.3-3.1.el8.x86_64 取代了 runc（由 runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64 提供）
     - 软件包 containerd.io-1.4.3-3.2.el8.x86_64 与 runc（由 runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64 提供）冲突
     - 软件包 containerd.io-1.4.3-3.2.el8.x86_64 取代了 runc（由 runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64 提供）
     - 软件包 containerd.io-1.4.4-3.1.el8.x86_64 与 runc（由 runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64 提供）冲突
     - 软件包 containerd.io-1.4.4-3.1.el8.x86_64 取代了 runc（由 runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64 提供）
     - 软件包 containerd.io-1.4.6-3.1.el8.x86_64 与 runc（由 runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64 提供）冲突
     - 软件包 containerd.io-1.4.6-3.1.el8.x86_64 取代了 runc（由 runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64 提供）
     - 软件包 containerd.io-1.4.8-3.1.el8.x86_64 与 runc（由 runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64 提供）冲突
     - 软件包 containerd.io-1.4.8-3.1.el8.x86_64 取代了 runc（由 runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64 提供）
     - 软件包 containerd.io-1.4.9-3.1.el8.x86_64 与 runc（由 runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64 提供）冲突
     - 软件包 containerd.io-1.4.9-3.1.el8.x86_64 取代了 runc（由 runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64 提供）
     - 无法为该任务安装最佳候选
     - 软件包 runc-1.0.0-56.rc5.dev.git2abd837.module_el8.3.0+569+1bada2e4.x86_64 被模块过滤过滤掉
     - 软件包 runc-1.0.0-66.rc10.module_el8.5.0+1004+c00a74f5.x86_64 被模块过滤过滤掉
     - 软件包 runc-1.0.0-72.rc92.module_el8.5.0+1006+8d0e68a2.x86_64 被模块过滤过滤掉
   (尝试在命令行中添加 '--allowerasing' 来替换冲突的软件包 或 '--skip-broken' 来跳过无法安装的软件包 或 '--nobest' 来不只使用软件包的最佳候选)
   [root@bogon ~]#
   
   # 再次执行安装命令
   >  yum install docker-ce docker-ce-cli containerd.io
   上次元数据过期检查：0:16:42 前，执行于 2021年11月24日 星期三 18时05分23秒。
   依赖关系解决。
   ===================================================================================
    软件包         架构   版本                                 仓库              大小
   ===================================================================================
   安装:
    containerd.io  x86_64 1.4.12-3.1.el8                       docker-ce-stable  28 M
    docker-ce      x86_64 3:20.10.11-3.el8                     docker-ce-stable  22 M
    docker-ce-cli  x86_64 1:20.10.11-3.el8                     docker-ce-stable  29 M
   安装依赖关系:
    container-selinux
                   noarch 2:2.167.0-1.module_el8.5.0+911+f19012f9
                                                               appstream         54 k
    docker-ce-rootless-extras
                   x86_64 20.10.11-3.el8                       docker-ce-stable 4.6 M
    docker-scan-plugin
                   x86_64 0.9.0-3.el8                          docker-ce-stable 3.7 M
    fuse-common    x86_64 3.2.1-12.el8                         baseos            21 k
    fuse-overlayfs x86_64 1.7.1-1.module_el8.5.0+890+6b136101  appstream         73 k
    fuse3          x86_64 3.2.1-12.el8                         baseos            50 k
    fuse3-libs     x86_64 3.2.1-12.el8                         baseos            94 k
    libcgroup      x86_64 0.41-19.el8                          baseos            70 k
    libslirp       x86_64 4.4.0-1.module_el8.5.0+890+6b136101  appstream         70 k
    slirp4netns    x86_64 1.1.8-1.module_el8.5.0+890+6b136101  appstream         51 k
   
   事务概要
   ===================================================================================
   安装  13 软件包
   
   总下载：89 M
   安装大小：372 M
   确定吗？[y/N]： y
   下载软件包：
   (1/13): container-selinux-2.167.0-1.module_el8.5.0  97 kB/s |  54 kB     00:00
   (2/13): libslirp-4.4.0-1.module_el8.5.0+890+6b1361 107 kB/s |  70 kB     00:00
   (3/13): fuse-overlayfs-1.7.1-1.module_el8.5.0+890+  98 kB/s |  73 kB     00:00
   (4/13): slirp4netns-1.1.8-1.module_el8.5.0+890+6b1 234 kB/s |  51 kB     00:00
   (5/13): fuse-common-3.2.1-12.el8.x86_64.rpm         45 kB/s |  21 kB     00:00
   (6/13): fuse3-3.2.1-12.el8.x86_64.rpm              102 kB/s |  50 kB     00:00
   (7/13): fuse3-libs-3.2.1-12.el8.x86_64.rpm         171 kB/s |  94 kB     00:00
   (8/13): libcgroup-0.41-19.el8.x86_64.rpm           208 kB/s |  70 kB     00:00
   (9/13): docker-ce-20.10.11-3.el8.x86_64.rpm        3.6 MB/s |  22 MB     00:06
   (10/13): docker-ce-rootless-extras-20.10.11-3.el8. 5.6 MB/s | 4.6 MB     00:00
   (11/13): docker-ce-cli-20.10.11-3.el8.x86_64.rpm   3.9 MB/s |  29 MB     00:07
   (12/13): docker-scan-plugin-0.9.0-3.el8.x86_64.rpm 1.8 MB/s | 3.7 MB     00:02
   (13/13): containerd.io-1.4.12-3.1.el8.x86_64.rpm   177 kB/s |  28 MB     02:44
   -----------------------------------------------------------------------------------
   总计                                               540 kB/s |  89 MB     02:47
   Docker CE Stable - x86_64                          2.2 kB/s | 1.6 kB     00:00
   导入 GPG 公钥 0x621E9F35:
    Userid: "Docker Release (CE rpm) <docker@docker.com>"
    指纹: 060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35
    来自: https://download.docker.com/linux/centos/gpg
   确定吗？[y/N]： y
   导入公钥成功
   运行事务检查
   事务检查成功。
   运行事务测试
   事务测试成功。
   运行事务
     准备中  :                                                                    1/1
     安装    : docker-scan-plugin-0.9.0-3.el8.x86_64                             1/13
     运行脚本: docker-scan-plugin-0.9.0-3.el8.x86_64                             1/13
     安装    : docker-ce-cli-1:20.10.11-3.el8.x86_64                             2/13
     运行脚本: docker-ce-cli-1:20.10.11-3.el8.x86_64                             2/13
     运行脚本: container-selinux-2:2.167.0-1.module_el8.5.0+911+f19012f9.noar    3/13
     安装    : container-selinux-2:2.167.0-1.module_el8.5.0+911+f19012f9.noar    3/13
     运行脚本: container-selinux-2:2.167.0-1.module_el8.5.0+911+f19012f9.noar    3/13
     安装    : containerd.io-1.4.12-3.1.el8.x86_64                               4/13
     运行脚本: containerd.io-1.4.12-3.1.el8.x86_64                               4/13
     运行脚本: libcgroup-0.41-19.el8.x86_64                                      5/13
     安装    : libcgroup-0.41-19.el8.x86_64                                      5/13
     运行脚本: libcgroup-0.41-19.el8.x86_64                                      5/13
     安装    : fuse3-libs-3.2.1-12.el8.x86_64                                    6/13
     运行脚本: fuse3-libs-3.2.1-12.el8.x86_64                                    6/13
     安装    : fuse-common-3.2.1-12.el8.x86_64                                   7/13
     安装    : fuse3-3.2.1-12.el8.x86_64                                         8/13
     安装    : fuse-overlayfs-1.7.1-1.module_el8.5.0+890+6b136101.x86_64         9/13
     运行脚本: fuse-overlayfs-1.7.1-1.module_el8.5.0+890+6b136101.x86_64         9/13
     安装    : libslirp-4.4.0-1.module_el8.5.0+890+6b136101.x86_64              10/13
     安装    : slirp4netns-1.1.8-1.module_el8.5.0+890+6b136101.x86_64           11/13
     安装    : docker-ce-rootless-extras-20.10.11-3.el8.x86_64                  12/13
     运行脚本: docker-ce-rootless-extras-20.10.11-3.el8.x86_64                  12/13
     安装    : docker-ce-3:20.10.11-3.el8.x86_64                                13/13
     运行脚本: docker-ce-3:20.10.11-3.el8.x86_64                                13/13
     运行脚本: container-selinux-2:2.167.0-1.module_el8.5.0+911+f19012f9.noar   13/13
     运行脚本: docker-ce-3:20.10.11-3.el8.x86_64                                13/13
     验证    : container-selinux-2:2.167.0-1.module_el8.5.0+911+f19012f9.noar    1/13
     验证    : fuse-overlayfs-1.7.1-1.module_el8.5.0+890+6b136101.x86_64         2/13
     验证    : libslirp-4.4.0-1.module_el8.5.0+890+6b136101.x86_64               3/13
     验证    : slirp4netns-1.1.8-1.module_el8.5.0+890+6b136101.x86_64            4/13
     验证    : fuse-common-3.2.1-12.el8.x86_64                                   5/13
     验证    : fuse3-3.2.1-12.el8.x86_64                                         6/13
     验证    : fuse3-libs-3.2.1-12.el8.x86_64                                    7/13
     验证    : libcgroup-0.41-19.el8.x86_64                                      8/13
     验证    : containerd.io-1.4.12-3.1.el8.x86_64                               9/13
     验证    : docker-ce-3:20.10.11-3.el8.x86_64                                10/13
     验证    : docker-ce-cli-1:20.10.11-3.el8.x86_64                            11/13
     验证    : docker-ce-rootless-extras-20.10.11-3.el8.x86_64                  12/13
     验证    : docker-scan-plugin-0.9.0-3.el8.x86_64                            13/13
   
   已安装:
     container-selinux-2:2.167.0-1.module_el8.5.0+911+f19012f9.noarch
     containerd.io-1.4.12-3.1.el8.x86_64
     docker-ce-3:20.10.11-3.el8.x86_64
     docker-ce-cli-1:20.10.11-3.el8.x86_64
     docker-ce-rootless-extras-20.10.11-3.el8.x86_64
     docker-scan-plugin-0.9.0-3.el8.x86_64
     fuse-common-3.2.1-12.el8.x86_64
     fuse-overlayfs-1.7.1-1.module_el8.5.0+890+6b136101.x86_64
     fuse3-3.2.1-12.el8.x86_64
     fuse3-libs-3.2.1-12.el8.x86_64
     libcgroup-0.41-19.el8.x86_64
     libslirp-4.4.0-1.module_el8.5.0+890+6b136101.x86_64
     slirp4netns-1.1.8-1.module_el8.5.0+890+6b136101.x86_64
   
   完毕！
   [root@bogon ~]#
   
   
   ```

   

4. 启动Docker

   ```shell
   # systemctl start docker
   [root@bogon ~]# ps -ef | grep docker
   root       41864       1  1 18:31 ?        00:00:00 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
   root       42024   38652  0 18:31 pts/2    00:00:00 grep --color=auto docker
   ```

   

5. 验证是否安装成功

   ```shell
   # docker run hello-world
   Unable to find image 'hello-world:latest' locally  # 本地未找到 hello-world:latest' 镜像
   latest: Pulling from library/hello-world # 拉取 library/hello-world 镜像
   2db29710123e: Pull complete # 拉取完成
   Digest: sha256:cc15c5b292d8525effc0f89cb299f1804f3a725c8d05e158653a563f15e4f685
   Status: Downloaded newer image for hello-world:latest
   
   Hello from Docker! # 看到这个 就说明成功了。
   This message shows that your installation appears to be working correctly.
   
   To generate this message, Docker took the following steps:
    1. The Docker client contacted the Docker daemon.
    2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
       (amd64)
    3. The Docker daemon created a new container from that image which runs the
       executable that produces the output you are currently reading.
    4. The Docker daemon streamed that output to the Docker client, which sent it
       to your terminal.
   
   To try something more ambitious, you can run an Ubuntu container with:
    $ docker run -it ubuntu bash
   
   Share images, automate workflows, and more with a free Docker ID:
    https://hub.docker.com/
   
   For more examples and ideas, visit:
    https://docs.docker.com/get-started/
   
   
   # docker ps -a
   CONTAINER ID   IMAGE         COMMAND    CREATED         STATUS                     PORTS     NAMES
   979eeabef746   hello-world   "/hello"   4 minutes ago   Exited (0) 4 minutes ago             elegant_elbakyan
   ```

   

6. 安装 docker-compose 参考 docker-compose章节。

7. xx

## 测试

```shell
# docker version
Emulate Docker CLI using podman. Create /etc/containers/nodocker to quiet msg.
Version:      3.3.1
API Version:  3.3.1
Go Version:   go1.16.7
Built:        Wed Nov 10 05:23:56 2021
OS/Arch:      linux/amd64


```



## 修改工作目录

docker安装时如果不指定目录（也就是工作目录），一般默认工作目录是 /var/lib/docker ，很多时候需要修改到大容量磁盘上进行存储，这里记录一下修改默认路径为 /data/docker 。

1. 添加并配置 /etc/docker/daemon.json 文件

   ```shell
   docker info |grep "Docker Root Dir" # 可查看目前的工作目录
   
   vim /etc/docker/daemon.json #如果不存在 就创建
   
   {
       "data-root": "/data/docker"
   }
   
   重启：  systemctl restart docker 
   
   ```

2. 修改systemd管理的docker服务文件 /usr/lib/systemd/system/docker.service 

   ```shell
   vim /usr/lib/systemd/system/docker.service
   
   # 新增 --data-root=/data/docker
   ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock --data-root=/data/docker
   
   
   
   # 重启
    systemctl daemon-reload 
   
    systemctl restart docker 
   ```

   

# 开机自启动

```shell
~ systemctl enable docker
Created symlink /etc/systemd/system/multi-user.target.wants/docker.service → /usr/lib/systemd/system/docker.service.

~ systemctl list-unit-files | grep enable | grep docker
docker.service                             enabled

```



# 基本命令

## docker run



## docker images

```shell
(base) [root@localhost ~]# docker images
REPOSITORY        TAG       IMAGE ID       CREATED         SIZE
redis             latest    40c68ed3a4d2   3 weeks ago     113MB
mongo             4.4.10    275f79a10a88   3 weeks ago     437MB
hello-world       latest    feb5d9fea6a5   2 months ago    13.3kB
mongo             3.6       2f21415cb85f   7 months ago    453MB
tikazyq/crawlab   latest    b2c11b2351e8   12 months ago   700MB
```



## docker ps

```shell
# docker ps -h
Flag shorthand -h has been deprecated, please use --help

Usage:  docker ps [OPTIONS]

List containers

Options:
  -a, --all             Show all containers (default shows just
                        running) # 展示所有的容器，默认仅显示 running 的容器。
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print containers using a Go template
  -n, --last int        Show n last created containers (includes
                        all states) (default -1) # 展示最新创建的容器 
  -l, --latest          Show the latest created container
                        (includes all states)
      --no-trunc        Don't truncate output
  -q, --quiet           Only display container IDs  # 仅仅展示 容器ID
  -s, --size            Display total file sizes 
  
```



```shell
# docker ps

#容器ID        镜像                      启动容器时运行的命令        容器的创建时间  容器状态    容器的端口信息和使用的连接类型 
CONTAINER ID   IMAGE                    COMMAND                  CREATED      STATUS      PORTS                                                                                  NAMES （#容器名称 ）
356791adbc84   tikazyq/crawlab:latest   "/bin/bash /app/dock…"   7 days ago   Up 7 days   8000/tcp, 8080/tcp                                                                     crawlab-worker-02
a5ff56008627   tikazyq/crawlab:latest   "/bin/bash /app/dock…"   7 days ago   Up 7 days   8000/tcp, 8080/tcp                                                                     crawlab-worker-01
f9499878e9ef   tikazyq/crawlab:latest   "/bin/bash /app/dock…"   7 days ago   Up 7 days   0.0.0.0:8000->8000/tcp, :::8000->8000/tcp, 0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   crawlab-master
05046c19feba   redis:latest             "docker-entrypoint.s…"   7 days ago   Up 7 days   0.0.0.0:6379->6379/tcp, :::6379->6379/tcp                                              redis
f4e17f96c88f   mongo:3.6                "docker-entrypoint.s…"   7 days ago   Up 7 days   0.0.0.0:27017->27017/tcp, :::27017->27017/tcp                                          mongo


STATUS: 容器状态。
  状态有7种：
  created（已创建）
  restarting（重启中）
  running 或 Up（运行中）
  removing（迁移中）
  paused（暂停）
  exited（停止）
  dead（死亡）
  
  
  
```

## docker exec 

- **-i:** 允许你对容器内的标准输入 (STDIN) 进行交互。
- **-t:** 在新容器内指定一个伪终端或终端。

```shell
# docker exec -it 05046c19feba /bin/bash 
root@05046c19feba:/data#  # 进去就是微型的Linux 了。

root@05046c19feba:/data# cat /proc/version
Linux version 4.18.0-348.2.1.el8_5.x86_64 (mockbuild@kbuilder.bsys.centos.org) (gcc version 8.5.0 20210514 (Red Hat 8.5.0-4) (GCC)) #1 SMP Tue Nov 16 14:42:35 UTC 2021
```

## docker logs

```shell
(base) [root@localhost ~]# docker logs 05046c19feba | head -n 5
1:C 02 Dec 2021 09:31:52.921 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 02 Dec 2021 09:31:52.921 # Redis version=6.2.6, bits=64, commit=00000000, modified=0, pid=1, just started
1:C 02 Dec 2021 09:31:52.921 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
1:M 02 Dec 2021 09:31:52.921 * monotonic clock: POSIX clock_gettime
1:M 02 Dec 2021 09:31:52.922 * Running mode=standalone, port=6379.
```

## docker stop

```shell
docker stop 05046c19feba 停止容器
```

## docker rm 

删除容器

## docker container prune

清理掉所有处于终止状态的容器。



## docker stats

```shell
(base) [root@localhost ~]# docker stats -h
Flag shorthand -h has been deprecated, please use --help

Usage:  docker stats [OPTIONS] [CONTAINER...]

Display a live stream of container(s) resource usage statistics

Options:
  -a, --all             Show all containers (default shows just
                        running) # 列出所有容器的状态
      --format string   Pretty-print images using a Go template
      --no-stream       Disable streaming stats and only pull the
                        first result # 只刷一次
      --no-trunc        Do not truncate output
```

```shell
(base) [root@localhost ~]# docker stats --no-stream
CONTAINER ID   NAME                CPU %     MEM USAGE / LIMIT     MEM %     NET I/O           BLOCK I/O         PIDS
356791adbc84   crawlab-worker-02   0.59%     141.3MiB / 7.507GiB   1.84%     4.68GB / 5.5GB    260MB / 137MB     19
a5ff56008627   crawlab-worker-01   0.56%     107.4MiB / 7.507GiB   1.40%     5.47GB / 6.59GB   35.5MB / 133MB    18
f9499878e9ef   crawlab-master      0.66%     177.2MiB / 7.507GiB   2.31%     5.79GB / 6.33GB   173MB / 149MB     18
05046c19feba   redis               0.20%     16.55MiB / 7.507GiB   0.22%     15.2GB / 8.04GB   37.9MB / 59.6MB   6
f4e17f96c88f   mongo               0.14%     2.848GiB / 7.507GiB   37.94%    2.62GB / 6.65GB   91.3MB / 27.8GB   53
(base) [root@localhost ~]#

```

# 镜像

## 创建 Python 镜像

1. 创建demo应用代码

```shell
$ cd /path/to/python-docker
$ pip3 install Flask
$ pip3 freeze | grep Flask >> requirements.txt
$ touch app.py

from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello , Docker Python ! '
    
    
```

2. 测试应用

   ```shell
   $ python3 -m flask run
   
   * Environment: production
      WARNING: This is a development server. Do not use it in a production deployment.
      Use a production WSGI server instead.
    * Debug mode: off
    * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
   127.0.0.1 - - [14/Dec/2021 17:48:59] "GET / HTTP/1.1" 200 -
   ```

3. 观察日志, 服务正常

   ```shell
   
   $ curl -i curl http://127.0.0.1:5000/
   
   Hello , Docker Python ! %
   curl: (6) Could not resolve host: curl
   HTTP/1.0 200 OK
   Content-Type: text/html; charset=utf-8
   Content-Length: 24
   Server: Werkzeug/2.0.2 Python/3.7.11
   Date: Tue, 14 Dec 2021 10:04:27 GMT
   
   ```

   

4. 创建Dockerfile

   ```shell
   $ touch Dockerfile
   $ vim Dockerfile
   
   # syntax=docker/dockerfile:1
   FROM python:3.8-slim-buster
   
   WORKDIR /app
   COPY requirements.txt requirements.txt
   RUN pip3 install -r requirements.txt
   
   COPY . .
   
   CMD ["python3", "-m", "flask", "run", "--host=0.0.0.0"]
   
   :wq!
   ```

   

5. 生成镜像

   ```shell
   $ tree
   .
   ├── Dockerfile
   ├── __pycache__
   │   └── app.cpython-37.pyc
   ├── app.py
   └── requirements.txt
   
   1 directory, 4 files
   
   
   $ docker build --tag python-docker .
   [+] Building 18.1s (14/14) FINISHED                                                  
    => [internal] load build definition from Dockerfile                            0.0s
    => => transferring dockerfile: 256B                                            0.0s
    => [internal] load .dockerignore                                               0.0s
    => => transferring context: 2B                                                 0.0s
    => resolve image config for docker.io/docker/dockerfile:1                      4.5s
    => docker-image://docker.io/docker/dockerfile:1@sha256:42399d4635eddd7a9b8a24  1.2s
    => => resolve docker.io/docker/dockerfile:1@sha256:42399d4635eddd7a9b8a24be87  0.0s
    => => sha256:42399d4635eddd7a9b8a24be879d2f9a930d0ed040a61324 2.00kB / 2.00kB  0.0s
    => => sha256:93f32bd6dd9004897fed4703191f48924975081860667932a4df 528B / 528B  0.0s
    => => sha256:e532695ddd93ca7c85a816c67afdb352e91052fab7ac19a6 1.21kB / 1.21kB  0.0s
    => => sha256:24a639a53085eb680e1d11618ac62f3977a3926fedf5b847 9.67MB / 9.67MB  0.9s
    => => extracting sha256:24a639a53085eb680e1d11618ac62f3977a3926fedf5b8471ace5  0.2s
    => [internal] load build definition from Dockerfile                            0.0s
    => [internal] load .dockerignore                                               0.0s
    => [internal] load metadata for docker.io/library/python:3.8-slim-buster       3.3s
    => [internal] load build context                                               0.0s
    => => transferring context: 912B                                               0.0s
    => [1/5] FROM docker.io/library/python:3.8-slim-buster@sha256:95db858dd3cb058  4.5s
    => => resolve docker.io/library/python:3.8-slim-buster@sha256:95db858dd3cb058  0.0s
    => => sha256:d78a351dcf4a1a6db24fb47be30bbe907dc2fdbd763249bf 7.92kB / 7.92kB  0.0s
    => => sha256:ffbb094f4f9e7c61d97c2b409f3e8154e2621a5074a008 27.15MB / 27.15MB  2.0s
    => => sha256:2f746edc7f5a90ff637067db7635196acdf5dd52be3b1a1a 2.77MB / 2.77MB  0.6s
    => => sha256:c994c68310f76ea392638ca5ad76459970e88e877963b8 10.72MB / 10.72MB  1.0s
    => => sha256:95db858dd3cb0587820edd001cb5fc46715529ed609e5634 1.86kB / 1.86kB  0.0s
    => => sha256:ab78639b9a19c806df52b44642e5e11ffd6ad01709c721b4 1.37kB / 1.37kB  0.0s
    => => sha256:e0cda2588a35cc1e1120f6eaf01828625149c8ccc852734efe9f 232B / 232B  1.2s
    => => sha256:8c7cc0114ed48565918cd34d4f37545ad4c2cf6b49002329 2.64MB / 2.64MB  1.4s
    => => extracting sha256:ffbb094f4f9e7c61d97c2b409f3e8154e2621a5074a0087d35f18  1.2s
    => => extracting sha256:2f746edc7f5a90ff637067db7635196acdf5dd52be3b1a1a3e207  0.2s
    => => extracting sha256:c994c68310f76ea392638ca5ad76459970e88e877963b8e6d2c1b  0.4s
    => => extracting sha256:e0cda2588a35cc1e1120f6eaf01828625149c8ccc852734efe9fd  0.0s
    => => extracting sha256:8c7cc0114ed48565918cd34d4f37545ad4c2cf6b4900232995f2c  0.2s
    => [2/5] WORKDIR /app                                                          0.1s
    => [3/5] COPY requirements.txt requirements.txt                                0.0s
    => [4/5] RUN pip3 install -r requirements.txt                                  4.1s
    => [5/5] COPY . .                                                              0.0s
    => exporting to image                                                          0.1s
    => => exporting layers                                                         0.1s
    => => writing image sha256:ccdc61301c7dc7fa723d7e659dc02cb16b742b174f62044ff6  0.0s
    => => naming to docker.io/library/python-docker                                0.0s
   
   Use 'docker scan' to run Snyk tests against images to find vulnerabilities and learn how to fix them
   ```

   

6. 查看本地镜像

   ```shell
   $ docker images | grep docker
   python-docker                        latest                                                  ccdc61301c7d   2 minutes ago   125MB
   ```

   

7. docker tag

   ```shell
   $ docker tag python-docker:latest python-docker:v1.0.0
   (py3-7)  xxx@ShaodeMacBook-Pro  ~/Works/study/docker  docker images
   REPOSITORY                           TAG                                                     IMAGE ID       CREATED          SIZE
   python-docker                        latest                                                  ccdc61301c7d   32 minutes ago   125MB
   python-docker                        v1.0.0                                                  ccdc61301c7d   32 minutes ago   125MB
   ```

   

8. Docker rmi  python-docker:v1.0.0   删除某个镜像

9. 运行镜像

   ```shell
   $ docker run python-docker
    * Environment: production
      WARNING: This is a development server. Do not use it in a production deployment.
      Use a production WSGI server instead.
    * Debug mode: off
    * Running on all addresses.
      WARNING: This is a development server. Do not use it in a production deployment.
    * Running on http://172.17.0.2:5000/ (Press CTRL+C to quit)
   
   
   $ docker run --publish 5001:5000 python-docker
    * Environment: production
      WARNING: This is a development server. Do not use it in a production deployment.
      Use a production WSGI server instead.
    * Debug mode: off
    * Running on all addresses.
      WARNING: This is a development server. Do not use it in a production deployment.
    * Running on http://172.17.0.2:5000/ (Press CTRL+C to quit)
   172.17.0.1 - - [15/Dec/2021 10:02:11] "GET / HTTP/1.1" 200 -
   172.17.0.1 - - [15/Dec/2021 10:02:11] "GET /favicon.ico HTTP/1.1" 404 -
   
   
   # 从宿主机已经可以访问到容器内的东西啦 ^_^
   $curl -i http://localhost:5001
   HTTP/1.0 200 OK
   Content-Type: text/html; charset=utf-8
   Content-Length: 24
   Server: Werkzeug/2.0.2 Python/3.8.12
   Date: Wed, 15 Dec 2021 10:03:08 GMT
   
   Hello , Docker Python ! %
   ```

   

10. 容器内运行一个mysql 

    1. 创建数据卷

       ```shell
       $ docker volume create mysql
       $ docker volume create mysql_config
       
       $ docker volume ls
       DRIVER    VOLUME NAME
       local     mysql
       local     mysql_config
       ```

       

    2. 创建网络

       ```shell
       $ docker network create mysqlnet
       
       $ docker network ls | grep mysqlnet
       de8481e859aa   mysqlnet   bridge    local
       ```

       

    3. 启动mysql 容器

       ```shell
       $ docker run --rm -d -v mysql:/var/lib/mysql \ # 数据卷映射
         -v mysql_config:/etc/mysql 
         -p 3306:3306 \ # 端口映射
         --network mysqlnet \ # 网络
         --name mysqldb \ # 容器名字
         -e MYSQL_ROOT_PASSWORD=p@ssw0rd1 \ # mysql root密码
         mysql # 镜像

    4. 查看

       ```shell
       $ docker ps | grep mysql
       db8da92bc2c8   mysql                                "docker-entrypoint.s…"   35 minutes ago   Up 35 minutes   0.0.0.0:3306->3306/tcp, 33060/tcp   mysqldb
       
       $ docker exec -it mysqldb mysql -u root -p 
       Enter password:
       Welcome to the MySQL monitor.  Commands end with ; or \g.
       Your MySQL connection id is 13
       Server version: 8.0.27 MySQL Community Server - GPL
       
       Copyright (c) 2000, 2021, Oracle and/or its affiliates.
       
       Oracle is a registered trademark of Oracle Corporation and/or its
       affiliates. Other names may be trademarks of their respective
       owners.
       
       Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
       
       mysql>
       ```

       

    5. 数据库容器已经准备完毕。

11. 应用连接mysql db

    ```python
    from os import curdir
    from flask import Flask
    import mysql.connector
    import json
    app = Flask(__name__)
    
    @app.route('/')
    def hello_world():
        return 'Hello , Docker Python ! '
    
    @app.route('/widgets')
    def get_widgets():
        mydb = mysql.connector.connect(
            host="mysqldb",
            user="root",
            password="p@ssw0rd1",
            database="inventory"
        )    
        cursor = mydb.cursor()
    
        cursor.execute("select * from widgets")
    
        row_headers = [x[0] for x in cursor.description]
    
        results = cursor.fetchall()
        json_data = []
        for result in results:
            json_data.append(dict(zip(row_headers, result)))
    
        cursor.close()
    
        return json.dumps(json_data)    
    
    @app.route('/initdb')
    def db_int():
        mydb = mysql.connector.connect(
            host="mysqldb",
            user="root",
            password="p@ssw0rd1",
        )
        cursor = mydb.cursor()
    
        cursor.execute("DROP DATABASE IF EXISTS inventory")
        cursor.execute("CREATE DATABASE inventory")
        cursor.close()
    
        mydb = mysql.connector.connect(
            host="mysqldb",
            user="root",
            password="p@ssw0rd1",
            database="inventory"
        )    
        cursor = mydb.cursor()
    
        cursor.execute("DROP TABLE IF EXISTS widgets")
        cursor.execute("CREATE TABLE widgets (name VARCHAR(255), description VARCHAR(255))")
        cursor.close()
    
        return 'init database'
    
    if __name__ == "__main__":
        app.run(host="0.0.0.0", port=5000)
            
    
    ```

    1. 安装  mysql-connector-python

       ```shell
       $ pip install mysql-connector-python
       
       ```

    2. 补充 requirements.txt

       ```shell
       $ pip freeze | grep mysql-connector-python >> requirements.txt
       ```

       

    3. 重新生成镜像

       ```shell
       docker build --tag python-docker-dev .
       ```

    4. 启动容器

       ```shell
        docker run \
         --rm -d \
         --network mysqlnet \
         --name rest-server \
         -p 5000:5000 \
         python-docker-dev
       ```

12. 检查结果

    ```shell
    $ curl http://localhost:5000/initdb # init database
    $ curl http://localhost:5000/widgets # []
    ```

    

13. 使用 docker-compose 参考 docker-compose 章节。[docker-compose](#docker-compose)





# 网络

```shell
ip addr / ifconfig  

# 当 Docker 启动时，会自动在主机上创建一个 docker0 虚拟网桥，实际上是 Linux 的一个 bridge，可以理解为一个软件交换机。它会在挂载到它的网口之间进行转发。
4: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default
    link/ether 02:42:c4:fc:bc:04 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:c4ff:fefc:bc04/64 scope link
       valid_lft forever preferred_lft forever
       
       
       
 ip addr | grep veth
235: veth3590bb4@if234: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master br-74a03f849e67 state UP group default
237: vetha1df05a@if236: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master br-74a03f849e67 state UP group default
245: veth7a7b191@if244: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master br-74a03f849e67 state UP group default
247: vethcd8240c@if246: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master br-74a03f849e67 state UP group default
249: veth3d8455d@if248: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master br-74a03f849e67 state UP group default      
       
```

![image-20211122171417196](/Users/xxx/Library/Application Support/typora-user-images/image-20211122171417196.png)

## 访问外网

```shell
# sysctl net.ipv4.ip_forward 
net.ipv4.ip_forward = 0 # 容器要想访问外部网络，需要本地系统的转发支持。在Linux 系统中，检查转发是否打开。

# https://stackoverflow.com/questions/44525301/docker-compose-3-sysctls-directive-unsupported
# This is because sysctl reads and modifies the attributes of the system kernel, so running it in a container does not make sense. It's logical that docker swarm does not support it anymore because if you have 2 stacks / docker-compose.yml all using this instruction, there would be conflict.

So the only way to do it is to run the command on the host machine.

# sysctl -w net.ipv4.ip_forward=1 容器内是禁止修改系统内核的。
sysctl: setting key "net.ipv4.ip_forward": Read-only file system 


# 回到宿主机执行 
# sysctl -w net.ipv4.ip_forward=1
net.ipv4.ip_forward = 1

# 修改完成之后，容器就可以进行外网访问了。
```

# docker-compose

1. 安装

   https://docs.docker.com/compose/install/

   ```shell
   > curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
     % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                    Dload  Upload   Total   Spent    Left  Speed
   100   633  100   633    0     0   1000      0 --:--:-- --:--:-- --:--:--  1000
   100 12.1M  100 12.1M    0     0  42526      0  0:04:59  0:04:59 --:--:-- 45037
   
   >  chmod +x /usr/local/bin/docker-compose
   ```

2. 查看版本

   ```shell
   # docker-compose version
   docker-compose version 1.29.2, build 5becea4c
   docker-py version: 5.0.0
   CPython version: 3.7.10
   OpenSSL version: OpenSSL 1.1.0l  10 Sep 2019
   
   
   ```

3. docker-compose 方式启动 python-docker 服务

   1. 当前目录新建docker-compose.dev.yml 文件

      ```yaml
      version: '3.8'
      
      services:
       web:
        build:
         context: .
        ports:
        - 5000:5000
        volumes:
        - ./:/app
      
       mysqldb:
        image: mysql
        ports:
        - 3306:3306
        environment:
        - MYSQL_ROOT_PASSWORD=p@ssw0rd1
        volumes:
        - mysql:/var/lib/mysql
        - mysql_config:/etc/mysql
      
      volumes:
        mysql:
        mysql_config:
      ```

      

   2. 启动

      ```shell
      $ docker-compose -f docker-compose.dev.yml up --build
      ```

   3. 测试

      ```shell
      $ curl http://localhost:5000/initdb
      $ curl http://localhost:5000/widgets
      ```

      

4. xx



## docker-compose.yml



## ps

## up

## down



# 常见问题

1. vi vim命令不存在

   ```shell
   # apt-get update
   # apt-get install vim
   # apt-get install telnet 等等类似
   
   apt-get update
   apt-get install iputils-ping # ping
   
   apt install -y iproute2  # ip
   ```

   

2. xx

3. xx



# podman

# K8S

## 基础

### 特点

1. 轻量级、消耗资源少
2. 开源
3. 弹性伸缩
4. 负载均衡：IPVS

### 概念

1. POD 概念
   1. Zizhu
2. 网络通讯



# Docker Elk

```shell
https://github.com/deviantony/docker-elk
```

## 1. 安装

```shell
 wget https://github.com/deviantony/docker-elk/archive/refs/heads/main.zip
 mv main.zip docker-elk.zip
 unzip docker-elk.zip

# 进入目录
cd docker-elk-main/

# 如果安装好了docker-compose 可以跳过下一步
 curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

 sudo chmod +x /usr/local/bin/docker-compose

#启动
 docker-compose up -d
 
 docker-compose up -d
Creating network "docker-elk-main_elk" with driver "bridge"
Creating volume "docker-elk-main_setup" with default driver
Creating volume "docker-elk-main_elasticsearch" with default driver
Building setup
Sending build context to Docker daemon  11.78kB
Step 1/7 : ARG ELASTIC_VERSION
Step 2/7 : FROM docker.elastic.co/elasticsearch/elasticsearch:${ELASTIC_VERSION}
8.1.1: Pulling from elasticsearch/elasticsearch
b37644e60321: Extracting [===============================================>   ]  30.47MB/31.79MB
b37644e60321: Pull complete
354d16e66fce: Pull complete
176322f4a06e: Pull complete
657b9785b0a4: Pull complete
9fbb74d281bc: Pull complete
2058679daf64: Pull complete
57aa30bb24b8: Pull complete
a5dbb6b35be5: Pull complete
0b9204bfafb2: Pull complete
Digest: sha256:2b75a3e2d4a296682dd598415b8921c931059562bbbf29c1e5c0d9c342d2a301
Status: Downloaded newer image for docker.elastic.co/elasticsearch/elasticsearch:8.1.1
 ---> ad7374c81155
Step 3/7 : USER root
 ---> Running in df1e62299009
Removing intermediate container df1e62299009
 ---> b78641b8e344
Step 4/7 : COPY . /
 ---> 8ac009aa323b
Step 5/7 : RUN set -eux; 	mkdir /state; 	chown elasticsearch /state; 	chmod +x /entrypoint.sh
 ---> Running in da23e8556099
+ mkdir /state
+ chown elasticsearch /state
+ chmod +x /entrypoint.sh
Removing intermediate container da23e8556099
 ---> 11f63b4425c2
Step 6/7 : USER elasticsearch:root
 ---> Running in 91900b938900
Removing intermediate container 91900b938900
 ---> 016bc77532ed
Step 7/7 : ENTRYPOINT ["/entrypoint.sh"]
 ---> Running in 1c2cde33cb19
Removing intermediate container 1c2cde33cb19
 ---> 9b487dfe7136
Successfully built 9b487dfe7136
Successfully tagged docker-elk-main_setup:latest
WARNING: Image for service setup was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Building elasticsearch
Sending build context to Docker daemon  4.608kB
Step 1/2 : ARG ELASTIC_VERSION
Step 2/2 : FROM docker.elastic.co/elasticsearch/elasticsearch:${ELASTIC_VERSION}
 ---> ad7374c81155
Successfully built ad7374c81155
Successfully tagged docker-elk-main_elasticsearch:latest
WARNING: Image for service elasticsearch was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Building logstash
Sending build context to Docker daemon  6.144kB
Step 1/2 : ARG ELASTIC_VERSION
Step 2/2 : FROM docker.elastic.co/logstash/logstash:${ELASTIC_VERSION}
8.1.1: Pulling from logstash/logstash
b37644e60321: Already exists
63650da27f21: Pull complete
010afd3d3417: Pull complete
9bd4dbe685a1: Pull complete
9f771a6be53c: Pull complete
769a900c2d41: Pull complete
219fb01d12f8: Pull complete
2d4f5b5a43e8: Pull complete
9cf866ea5379: Pull complete
6217d881ff54: Pull complete
0cf9245e2503: Pull complete
Digest: sha256:a09e0272969413b27ba1c6de2b1efecf73175f1a5b5a4d559f090cc5c36b6378
Status: Downloaded newer image for docker.elastic.co/logstash/logstash:8.1.1
 ---> 9669367812d1
Successfully built 9669367812d1
Successfully tagged docker-elk-main_logstash:latest
WARNING: Image for service logstash was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Building kibana
Sending build context to Docker daemon  4.608kB
Step 1/2 : ARG ELASTIC_VERSION
Step 2/2 : FROM docker.elastic.co/kibana/kibana:${ELASTIC_VERSION}
8.1.1: Pulling from kibana/kibana
b37644e60321: Already exists
0cbdd2e12c63: Pull complete
0f736544c452: Pull complete
ead9e8815713: Pull complete
07a78adf1047: Pull complete
8deae88e9fa2: Pull complete
178902b0f17d: Pull complete
993ff4cd0b25: Pull complete
d0e16a001008: Pull complete
184b7127f82f: Pull complete
4cc04c22891d: Pull complete
a27502b5fdfd: Pull complete
9724e2d22e6c: Pull complete
Digest: sha256:9a402a5acd270c18cf25b7c83185c5fa08ca575d3243cf0edbf54cee397d7ecb
Status: Downloaded newer image for docker.elastic.co/kibana/kibana:8.1.1
 ---> d567696c16b3
Successfully built d567696c16b3
Successfully tagged docker-elk-main_kibana:latest
WARNING: Image for service kibana was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Creating docker-elk-main_setup_1         ... done
Creating docker-elk-main_elasticsearch_1 ... done
Creating docker-elk-main_kibana_1        ... done
Creating docker-elk-main_logstash_1      ... done


```











## 














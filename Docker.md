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



# 基本命令



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

![image-20211122171417196](/Users/shaogaojie/Library/Application Support/typora-user-images/image-20211122171417196.png)

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

   

3. x

4. xx

5. xx

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


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

4. 
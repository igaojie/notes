# 常见问题

1、ssh 登录会话保持时间太短解决方法：

```shell
vim /etc/ssh/sshd_config

ClientAliveInterval 60
ClientAliveCountMax 3

service sshd reload 重启服务
```


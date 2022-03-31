# ClashX

## 配置规则

```shell
cd ~/.config/clash/

vim proxyIgnoreList.plist

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<array>
        <string>192.168.0.0/16</string>
        <string>10.0.0.0/8</string>
        <string>172.16.0.0/12</string>
        <string>127.0.0.1</string>
        <string>localhost</string>
        <string>*.local</string>
        <string>*.crashlytics.com</string>
        <string>*.xxxxxx.com</string> # 如果想把某个域名加入白名单 不走vpn 可以在这里添加 然后重启ClashX即可。
</array>
</plist>
```


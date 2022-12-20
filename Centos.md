# Centos

## 常见问题

1、OpenAI\Exceptions\TransporterException with message 'cURL error 60: SSL certificate problem: certificate has expired (see https://curl.haxx.se/libcurl/c/libcurl-errors.html) for https://api.openai.com/v1/completions'

```shell
原因就是 /etc/ssl/certs 目录下的crt文件需要更新。

# ll -t
总用量 236
lrwxrwxrwx 1 root root     49 12月 16 15:54 ca-bundle.crt -> /etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem
lrwxrwxrwx 1 root root     55 12月 16 15:54 ca-bundle.trust.crt -> /etc/pki/ca-trust/extracted/openssl/ca-bundle.trust.crt
-rwxr-xr-x 1 root root    610 12月 17 2020 make-dummy-cert
-rw-r--r-- 1 root root   2516 12月 17 2020 Makefile
-rwxr-xr-x 1 root root    829 12月 17 2020 renew-dummy-cert


操作步骤
yum install -y ca-certificates

update-ca-trust force-enable

update-ca-trust
```



2、asd

3、sd
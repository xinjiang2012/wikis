# 在Oracle Linux7（OL7）上安装docker


https://confluence.oraclecorp.com/confluence/display/OSDC/Docker+-+Install+on+Oracle+Linux+7


### 设置代理
On Oracle Linux 7, Docker is handled by systemd.

Typically the proxy settings will be located in /etc/systemd/system/docker.service.d/http-proxy.conf and look something like this:
```
[Service]
Environment="HTTP_PROXY=http://www-proxy.us.oracle.com:80/"
Environment="HTTPS_PROXY=http://www-proxy.us.oracle.com:80/"
Environment="NO_PROXY=localhost,127.0.0.1,.us.oracle.com"
```

If the file doesn't exists, you can just create it! Systemd will source all the files under /etc/systemd/system/docker.service.d/ automatically. You can also name the file differently. For the sake of simplicity, I would suggest to use /etc/systemd/system/docker.service.d/http-proxy.conf

Then restart the docker process using the following command:
```shell
$> sudo systemctl daemon-reload
$> sudo systemctl restart docker
```
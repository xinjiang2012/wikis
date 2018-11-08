# 解决Docker无法访问私有仓库问题

在某些情况下，你需要搭建自己的私有Docker仓库，并且通过https访问。此时，你会遇到如下的x509问题:
```language
[hudson@slcn03vmf0419 ~]$ sudo docker run -dP cloudqa.docker.oraclecorp.com/cloudqa/higgs_watchdog:1.0
Unable to find image 'cloudqa.docker.oraclecorp.com/cloudqa/higgs_watchdog:1.0' locally
Trying to pull repository cloudqa.docker.oraclecorp.com/cloudqa/higgs_watchdog ...
docker: Get https://cloudqa.docker.oraclecorp.com/v2/: x509: certificate is valid for artifactory-slc.oraclecorp.com, artifactory.oraclecorp.com, almrepo.us.oracle.com, not cloudqa.docker.oraclecorp.com.
See 'docker run --help'.
```

解决方式就是把改站点加入加入可信任的列表，使用root用户执行如下命令:
```shell
$ export DOMAIN_NAME=<Domain>
$ export TCP_PORT=<port number>
$ openssl s_client -connect $DOMAIN_NAME:$TCP_PORT -showcerts </dev/null 2>/dev/null | openssl x509 -outform PEM | tee /etc/pki/ca-trust/source/anchors/$DOMAIN_NAME.crt
$ update-ca-trust
$ /bin/systemctl restart docker.service
```

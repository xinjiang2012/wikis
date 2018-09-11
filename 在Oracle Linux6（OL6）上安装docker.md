# 在Oracle Linux6（OL6）上安装docker

1. 获取最新的yum repo
```shell
# wget http://public-yum.oracle.com/public-yum-ol6.repo -P /etc/yum.repos.d 
```
2. 创建一个新的repo文件/etc/yum.repos.d/docker.repo
```shell
# cat /etc/yum.repos.d/docker.repo
[dockerrepo]
name=Docker Repository
baseurl=https://yum.dockerproject.org/repo/main/oraclelinux/6
enabled=1
gpgcheck=1
gpgkey=https://yum.dockerproject.org/gpg
```
3. 编辑/etc/login.defs文件,增加UID_MIN和GID_MIN的值
```
UID_MIN                   1000
GID_MIN                   1000
```
4. 启用repo[ol6_uekr4]在public-yum-ol6.repo文件中
```
[ol6_UEKR4]
name=Latest Unbreakable Enterprise Kernel Release 4 for Oracle Linux $releasever ($basearch)
baseurl=https://yum.oracle.com/repo/OracleLinux/OL6/UEKR4/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=1
```
5. 更新linux kernel到 3.10.0或更高版本，然后重启
```shell
# yum update kernel-uek
```
6. 安装docker
```shell
# yum install docker-engine -y
```
7. 编辑 /etc/init.d/docker
```
修改以下行
"$unshare" -m -- $exec -d $other_args &>> $logfile &
为
$exec  $other_args &>> $logfile &
```
8. 启动docker
```shell
# service docker start
```
9. 检查状态
```language
# service docker status
并且检查log文件 /var/log/docker 是否有错
```
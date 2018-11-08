# Docker使用经验总结

### 如果enable了SSHD服务，环境变量需要写入文件，如/etc/profile
```
RUN echo "export DISPLAY=:0" >> /etc/profile
```

### 使用 Supervisor 来管理Container里的进程服务
Docker 不是虚拟机，容器中的应用都应该以前台执行，而不是像虚拟机、物理机里面那样，用 upstart/systemd 去启动后台服务，容器内没有后台服务的概念。

一些初学者将 CMD 写为：

CMD service nginx start
然后发现容器执行后就立即退出了。甚至在容器内去使用 systemctl 命令结果却发现根本执行不了。这就是因为没有搞明白前台、后台的概念，没有区分容器和虚拟机的差异，依旧在以传统虚拟机的角度去理解容器。

对于容器而言，其启动程序就是容器应用进程，容器就是为了主进程而存在的，主进程退出，容器就失去了存在的意义，从而退出，其它辅助进程不是它需要关心的东西。

而使用 service nginx start 命令，则是希望 upstart 来以后台守护进程形式启动 nginx 服务。而刚才说了 CMD service nginx start 会被理解为 CMD [ "sh", "-c", "service nginx start"]，因此主进程实际上是 sh。那么当 service nginx start 命令结束后，sh 也就结束了，sh 作为主进程退出了，自然就会令容器退出。

正确的做法是直接执行 nginx 可执行文件，并且要求以前台形式运行。比如：

CMD ["nginx", "-g", "daemon off;"]

### Dockerfile 最佳实践
https://yeasy.gitbooks.io/docker_practice/appendix/best_practices.html#dockerfile-%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5
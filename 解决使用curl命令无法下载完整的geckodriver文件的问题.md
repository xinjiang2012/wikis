# 解决使用curl命令无法下载完整的geckodriver文件的问题

最近在使用Selenium做自动化测试，需要下载geckodriver。通过官网https://github.com/mozilla/geckodriver/releases/ 找到下载文件的link，使用curl command无法完整下载文件。这是因为当选择文件下载的link下载文件时，被重定向到实际的下载地址AWS的S3去了。可以使用浏览器下载文件，在“下载”页面查看实际的下载地址。
```shell
$ $curl -o geckodriver-linux64.tar.gz https://github.com/mozilla/geckodriver/releases/download/v0.23.0/geckodriver-v0.23.0-linux64.tar.gz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   624    0   624    0     0    348      0 --:--:--  0:00:01 --:--:--   348`
```
从上面看出下载的文件只有624 byte,而实际文件37MB.
下面是在浏览器下载页面找到的实际下载地址.
```
https://github-production-release-asset-2e65be.s3.amazonaws.com/25354393/6ec7f500-c7e0-11e8-8c0f-77179f969aef?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20181030%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20181030T075504Z&X-Amz-Expires=300&X-Amz-Signature=209185bf8c79430c87e23e8df4eb8fabeb76afdd9416294b066b994bb36a8877&X-Amz-SignedHeaders=host&actor_id=4082092&response-content-disposition=attachment%3B%20filename%3Dgeckodriver-v0.23.0-linux64.tar.gz&response-content-type=application%2Foctet-stream
```
可以通过使用-L 跟随链接重定向来解决。此时我们想要curl做的，就是像浏览器一样跟随链接的跳转，获取最终的网页内容。例如：
```shell
$ curl -L -o geckodriver-linux64.tar.gz https://github.com/mozilla/geckodriver/releases/download/v0.23.0/geckodriver-v0.23.0-linux64.tar.gz
```
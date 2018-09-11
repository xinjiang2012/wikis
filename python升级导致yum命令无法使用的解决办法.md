# python升级导致yum命令无法使用的解决办法

系统执行yum upgrade操作后， 当运行yum命令时会遇到如下错误信息：

```shell
$ yum
There was a problem importing one of the Python modules
required to run yum. The error leading to this problem was:

   No module named yum

Please install a package which provides this module, or
#!/usr/bin/python2.7
verify that the module is installed correctly.

It's possible that the above module doesn't match the
current version of Python, which is:
2.7.13 (default, Sep  8 2017, 07:48:02)
[GCC 4.4.7 20120313 (Red Hat 4.4.7-18)]

If you cannot solve this problem yourself, please go to
the yum faq at:
  http://yum.baseurl.org/wiki/Faq
```

错误原因：错误信息描述为 yum 所依赖的python 不相符，请安装相对应的python即可

解决过程：
1. 查看python版本,可以看出安装了两个版本的python
```shell
$ whereis python
python: /usr/bin/python2.6-config /usr/bin/python2.7 /usr/bin/python2.7-config /usr/bin/python2.6 /usr/bin/python /usr/lib/python2.7 /usr/lib/python2.6 /usr/lib64/python2.6 /usr/local/bin/python2.7 /usr/local/bin/python2.7-config /usr/local/bin/python /usr/include/python2.7 /usr/include/python2.6 /usr/local/python2.7 /usr/share/man/man1/python.1.gz /usr/share/man/man1/python2.1 /usr/share/man/man1/python.1
```
2. 查看默认Python版本，说明yum调用了高版本的Python
```shell
$ python -V
Python 2.7.13
```
3. 修改yum文件，引用低版本的python
```shell
$ cat /usr/bin/yum  | grep python
#!/usr/bin/python2.6
```
4. 执行yum命令验证是否正确运行


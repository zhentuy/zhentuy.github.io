---
layout: post
title: "linux 删除过期文件"
date: 2015-12-31 22:11:15 +0800
comments: true
categories: 
---

项目中需要定期清理过期的图片，开始用:
    find /var/tmp/stuff -mtime +90 -exec /bin/rm {} \;
这个每找到一个文件就启动一个新的进程，慢慢悠悠。

于是发明了改进型I:
    /bin/rm $(find /var/tmp/stuff -mtime +90 -print)
这下就执行两个进程快是快了，但是由于文件太多，命令超长了！

改进型II：
    find /var/tmp/stuff -mtime +90 -print | xargs /bin/rm
这下日子好过了，N天后磁盘又报警了，纳尼! 原因是xargs是按空格分隔参数的，
如果你文件名中有空格，它当成两个文件删了，当然神马都没删掉

改进型III:  
find  的 print0  action 用 askii 的 NUL 做分隔符  
xargs 的 -0 选项用 NULL做分隔符。用NUL 做分隔符的改进：
    find /var/tmp/stuff -mtime +90 -print0 | xargs -0 /bin/rm
但是这个不够安全，可能被越权删除

终极版来了：
    find /var/tmp/stuff -mtime +90 -delete
简单安全快速

不要问我从哪里学的，，给我发红包告诉你  
不要告诉别人你 info  find 看下

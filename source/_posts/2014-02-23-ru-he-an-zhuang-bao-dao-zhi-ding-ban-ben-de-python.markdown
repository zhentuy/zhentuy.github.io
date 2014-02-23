---
layout: post
title: "如何安装包到指定版本的python"
date: 2014-02-23 15:35:24 +0800
comments: true
categories: devenv
---

在centos6.3下装了python2.7.3 系统自带python2.6。

    [zhu@star sf]$ which pip
    /usr/local/bin/pip
    [zhu@star sf]$ sudo which pip
    /usr/bin/pip
    [zhu@star sf]$ which python
    /usr/local/bin/python
    [zhu@star sf]$ sudo which python
    /usr/bin/python
用which命令看到sudo后的pip已经不是我们想用的pip了。如果sudo pip install 会直接安装2.6中。要想装包到2.7中其实很简单： *用绝对路径*
    [zhu@star sf]$ sudo /usr/local/bin/pip install requests 
    Downloading/unpacking requests
    Downloading requests-2.2.1-py2.py3-none-any.whl (625kB): 625kB downloaded
    Installing collected packages: requests
    Successfully installed requests
    Cleaning up...
    [zhu@star sf]$ pip show requests
    ---
    Name: requests
    Version: 2.2.1
    Location: /usr/local/lib/python2.7/site-packages
    Requires:

从pip show 的信息中看出requests 这个包已经装入到2.7的目录了。

如果想要明确的指定装包到2.6还有一个pip2.6，只需要用pip2.6替代pip就把包装入py2.6了
    [zhu@star sf]$ which pip2.6
    /usr/bin/pip2.6

---
layout: post
title: "python 编码"
date: 2014-08-31 07:57:18 -0700
comments: true
categories: python
---
首先要搞清楚unicode和encoding之间的关系。unicode定义了一个字符的“值”，不管几进制就一个整数对应一个字符。
但是这个值怎么存，unicode不管。  
“这不是我的事” --unicode  
于是出来utf8 gbk utf16 等编码(encoding)它们的工作是把unicode转换成byte二进制，这样就能存能取了。

> The rules for translating a Unicode string into a sequence of bytes are called an encoding.

一般编码不会包括所有unicode，这就是为啥我们在python里面经常遇到头痛的UnicodeDecodeError,UnicodeEncodeError
的异常了。这两个异常一般带个信息'xxx' codec cant encode(decode) characters(bytes) in position n-m。说明这几个
unicode我没有编码，或者这段二进制数据按我的编码解析不成unicode。

* python 2.x 中str表示8位文本和二进制数据。 unicode 表示宽字符unicode
* python 3.x 中str表示8或者更宽的unicode文本，byte表示二进制数据

在python2.6的下unicode通过encode转换成str，str通过decode转换成unicode.  
看到系统默认的encoding是utf8
    [zhu@zhu zhentuy.github.io]$ env | grep LANG
    LANG=en_US.UTF-8
-----

    In [7]: s = 'abc朱海鹏' 
    In [8]: us = u'abc朱海鹏'
    In [9]: s
    Out[9]: 'abc\xe6\x9c\xb1\xe6\xb5\xb7\xe9\xb9\x8f'
    In [10]: us
    Out[10]: u'abc\u6731\u6d77\u9e4f'
    In [11]: len(s)
    Out[11]: 12
    In [12]: len(us)
    Out[12]: 6
我们看到str的数据在abc后面就是\x 十六进制了，而且长度是按照二进制来计算的
us的\u开头表明是unicode，len返回的长度是6二进制下返回12, 'abc'占3byte 
可以计算出这三个汉字在utf8编码下每个占3byte，
    In [13]: s.decode('utf8')
    Out[13]: u'abc\u6731\u6d77\u9e4f'
    In [14]: s.decode('utf8') == us
    Out[14]: True
    In [15]: us.encode('utf8')
    Out[15]: 'abc\xe6\x9c\xb1\xe6\xb5\xb7\xe9\xb9\x8f'
    In [16]: us.encode('utf8') == s
    Out[16]: True
二进制str与unicode之间的转换通过decode与encode
    In [17]: print us, s
    abc朱海鹏 abc朱海鹏
    In [18]: print us.encode('gbk')
    abc▒▒
    In [19]: print s.decode('gbk')
    ---------------------------------------------------------------------------
    UnicodeDecodeError                        Traceback (most recent call last)
    /opt/hub/zhentuy.github.io/<ipython-input-19-a2cf99eaf4f2> in <module>()
    ----> 1 print s.decode('gbk')

    UnicodeDecodeError: 'gbk' codec can't decode byte 0x8f in position 11: incomplete multibyte sequence
gbk 肯定给这三个常用汉字编码了，这个ipython里面用的utf8来识别宽字符，如果你在win下shell里面会看到(cmd里面启动
python不要用IDLE)
    >>> s = "朱海鹏"
    >>> us = u"朱海鹏"
    >>> s
    '\xd6\xec\xba\xa3\xc5\xf4'
    >>> us
    u'\u6731\u6d77\u9e4f'
    >>> print s,us
    朱海鹏 朱海鹏
    >>> print us.encode('gbk')
    朱海鹏
    >>> print us.encode('utf8')
    鏈辨捣楣
    >>> print us.encode('gb2312')
    朱海鹏
乱码是因为系统默认编码(encoding)的问题，不是因为unicode字符‘朱海鹏’不能编码成gbk, gb2312。

unicode是不能直接保存到文件的，存到文件系统的只能是二进制，由于二进制肯定是某种编码的二进制，所以读取的时候要对上号
    In [20]: f = open('tunisave.txt', 'w')
    In [21]: f.write(s)
    In [22]: f.write(us)
    ---------------------------------------------------------------------------
    UnicodeEncodeError                        Traceback (most recent call last)
    /opt/hub/zhentuy.github.io/<ipython-input-22-a2427ff060ce> in <module>()
    ----> 1 f.write(us)

    UnicodeEncodeError: 'ascii' codec can't encode characters in position 3-5: ordinal not in range(128)
    In [23]: f.write(us.encode('gbk'))
    In [24]: f.close()
    In [25]: f = open('tunisave.txt', 'r')
    In [26]: data = f.read()
    In [27]: data
    Out[27]: 'abc\xe6\x9c\xb1\xe6\xb5\xb7\xe9\xb9\x8fabc\xd6\xec\xba\xa3\xc5\xf4'
    In [28]: print data
    abc朱海鹏abc▒▒
    In [29]: print data.decode('gbk')
    abc鏈辨捣楣廰bc朱海鹏
后面乱码是因为uft8格式和gbk格式的二进制数据混着写了~~

[阮一峰这篇博客讲的不错，还讲了utf8怎么编码](http://www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html)  
[python howto unicode 讲的很清楚](https://docs.python.org/2/howto/unicode.html)

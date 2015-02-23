---
layout: post
title: "python导入的路径，绝对导入，相对导入"
date: 2014-08-26 07:39:03 -0700
comments: true
categories: python 
---

**首先要区分的是两种导入：**  
1.  包内导入  
2.  在python yourcode 运行时导入 我把这个命名为“直接导入”  
相对导入，绝对导入这些概念仅作用于包内导入  

不管哪种导入python需要找到它们  
python模块导入时的搜索路径：  
1.   程序主目录，执行程序是包含执行代码文件的目录，交互模式下为当前工作目录,优先级最高  
2.   PYTHONPATH中的目录  
3.   标准链接库目录，就是python的安装目录，源码在里面  
4.   3.x 中可以用.pth 文件  

这些构成了sys.path。你写的模块存储路径在sys.path 里面就可以import。
由于搜索路径有先后顺序，所以前面同名的模块容易“挡住”后面的模块  
比如dir0目录内有main.py和string.py两个文件，在main.py里面导入string：
>目录结构  
>dir0:  
>----main.py   
>----string.py  

    #string.py代码:
    print 'importing string'
    AAA = 10

    #main.py 代码
    import string
    print '****in main****'
    print dir(string)

这时候在dir0目录下运行`python main.py`
    [zhu@zhu dir0]$ python main.py
    importing string
    ****in main****
    ['AAA', '__builtins__', '__doc__', '__file__', '__name__', '__package__']
明显导入了主目录的string而没有导入标准库的，因为导入的时候主目录优先级高   
可以尝试换一个目录执行main.py把sys.path打印出来。能看到sys.path第一个目录就是“盛住”main.py的目录
所以自己写模块的起名要小心，不要覆盖了标准库

####包内导入
包是python组织代码的方式，如果一个目录下有__init__.py文件，python就会认为这是一个package。
如果这个目录的上一级目录在sys.path里面，这个目录下的py文件以及子目录（必须有__init__.py) 下的文件就可以被python 导入了 
    import dir0.dir1.mod
    import dir0.dir1.dir2.mod
在python2.6及以前，在一个包内导入相同目录下的模块优先级最高，享受直接导入的主目录下的待遇.
但是这时候我要导入标准库的同名模块，不要导入“同包”的模块怎么办？  
python3.0开始包内导入默认实行绝对导入，即在dir0.dir1的py文件里面执行`import string`不考虑“同包”的string模块  
如果要导入同包的string用明确的 from . import string语法，这样能精确控制了。   
2.5-2.7可以通过from __future__ import absolute_import 改变包导入的行为为绝对导入  
写代码测试一下，先把dir0放到PYTHONPATH里面`export PYTHONPATH=/home/zhu/study/learn/package/dir0/`  
测试包导入的目录结构：
>dir0:  
>----dir1:  
>--------\__init__.py  
>--------mod.py   
>--------string.py   

    # mod.py
    from __future__ import absolute_import 
    print '----in dir0.mod----'
    import string
    print dir(string)
    
在自己的home目录下写一个导入dir1.mod 的测试代码test_import.py
    import dir1.mod
代码`python test_import.py`运行结果：
    [zhu@zhu ~]$ python test_import.py
    ----in dir0.mod----
    ['Formatter', 'Template', '_TemplateMetaclass', '__builtins__', '__doc__', '__file__', '__name__', '__package__', '_float', '_idmap', '_idmapL', '_int', '_long', '_multimap', '_re', 'ascii_letters', 'ascii_lowercase', 'ascii_uppercase', 'atof', 'atof_error', 'atoi', 'atoi_error', 'atol', 'atol_error', 'capitalize', 'capwords', 'center', 'count', 'digits', 'expandtabs', 'find', 'hexdigits', 'index', 'index_error', 'join', 'joinfields', 'letters', 'ljust', 'lower', 'lowercase', 'lstrip', 'maketrans', 'octdigits', 'printable', 'punctuation', 'replace', 'rfind', 'rindex', 'rjust', 'rsplit', 'rstrip', 'split', 'splitfields', 'strip', 'swapcase', 'translate', 'upper', 'uppercase', 'whitespace', 'zfill']

明显是标准库里的string

修改dir1.dir0.mod.py:
    # mod.py
    from __future__ import absolute_import 
    print '----in dir0.mod----'
    from . import string
    print dir(string)
这时候的输出：
    ----in dir0.mod----
    imporing string
    ['AAA', '__builtins__', '__doc__', '__file__', '__name__', '__package__']
这时候导入了包内的string

还有相对导入不能放到直接运行的代码中，如果执行`python dir1/mod.py`会得到：
    ----in dir0.mod----
    Traceback (most recent call last):
      File "dir1/mod.py", line 4, in <module>
      from . import string
    ValueError: Attempted relative import in non-package
如果要测试有相对导入的模块可以用`python -m yourmod`。

困惑我最久的就是包导入和普通导入的区分

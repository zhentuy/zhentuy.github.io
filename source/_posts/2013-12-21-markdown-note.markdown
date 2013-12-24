---
layout: post
title: "markdown note"
date: 2013-12-21 05:34:26 -0800
comments: true
categories: 
---

<div>
块元素必须在前后加上空行与其它内容区隔开，还要求它们的开始标签与结尾标签不能用制表符或空格来缩进
*在 HTML 区块标签间的 Markdown 格式语法将不会被处理*
和处在 HTML 区块标签间不同，Markdown 语法在 HTML 区段标签间是有效的。
需要转意的字符 “<“ ”&“  md回自动转移，或者不转 code 范围内除外
code 范围内，不论是行内还是区块， < 和 & 两个符号都一定会被转换成 HTML 实体，
一个 Markdown 段落是由一个或多个连续的文本行组成，它的前后要有一个以上的空行
段内强迫换行在插入处先按入两个以上的空格然后回车

强制  
换行
============
bb
--------
</div>

*在 HTML 区块标签间的 Markdown 格式语法将不会被处理*
一个 Markdown 段落是由一个或多个连续的文本行组成，它的前后要有一个以上的空行
强制  
换行

============
bb
--------

# 1
## 2
### 3
#### 4
#####5
###### 6

#块引用：
>这是一块
折行第二行

>second  no fold
> next line
>> nested
>引用的区块内也可以使用其他的 Markdown 语法
>## H2
>code:
>
>  print "tanky woo is sb"

无序列表使用星号、加号或是减号作为列表标记：

- 1
-2
-3
- 4

有序列表则使用数字接着一个英文句点：
1. 1
2. 2
3.3

1.   这是一段PA
PA第二行
2. PB
PB第二行

### 带段落的列表
*   段落

    中断

    后段

*   第二段
        print 'wulei chaotang'

代码段
    def soeasy():
        return 0

   <div class="footer">
           &copy; 2004 Foo Corporation
   </div>

 分割线
 - - - 
代码块中一般的markdown语法不会被转换

[这是一个链接](http://www.163.com "title")指向163

[用变量定义的连接][myblog]

[myblog]: http://zhentuy.github.io/ "github博客地址"

**[myblog][] 隐式连接就是省略了链接字符**
\***aa**
```print abc``d````
A single backtick in a code span: `` ` ``

A backtick-delimited string in a code span: `` `foo` ``
<div>
块元素必须在前后加上空行与其它内容区隔开，还要求它们的开始标签与结尾标签不能用制表符或空格来缩进
</div>
<div>
块元素必须在前后加上空行与其它内容区隔开，还要求它们的开始标签与结尾标签不能用制表符或空格来缩进
</div>

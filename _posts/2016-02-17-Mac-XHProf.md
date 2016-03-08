---
title: Mac下XHProf安装与使用
title-en: use XHProf on Mac
date: 2016-02-17 17:45:25 +0800
categories: PHP 
tags: mac xhprof php
layout: post
---


### Mac下XHProf安装与使用
> XHProf 是一个轻量级的分层性能测量分析器。 在数据收集阶段，它跟踪调用次数与测量数据，展示程序动态调用的弧线图。 它在报告、后期处理阶段计算了独占的性能度量，例如运行经过的时间、CPU 计算时间和内存开销。 函数性能报告可以由调用者和被调用者终止。 在数据搜集阶段 XHProf 通过调用图的循环来检测递归函数，通过赋予唯一的深度名称来避免递归调用的循环。
> XHProf 包含了一个基于 HTML 的简单用户界面(由 PHP 写成)。 基于浏览器的用户界面使得浏览、分享性能数据结果更加简单方便。 同时也支持查看调用图。
> XHProf 的报告对理解代码执行结构常常很有帮助。 比如此分层报告可用于确定在哪个调用链里调用了某个函数。
> XHProf 对两次运行进行比较（又名 "diff" 报告），或者多次运行数据的合计。 对比、合并报告，很像针对单次运行的“平式视图”性能报告，就像“分层式视图”的性能报告。

1. 下载XHProf
	[XHProf](http://pecl.php.net/package/xhprof "XHProf")
2. 使用phpize编译XHProf
    
```
$ cd Documents/MyDesktop/xhprof-0.9.4/xhprof-0.9.4/extension 
$ phpize
$ ./configure
$ make 
$ make instal                                   
```

将编译成功的xhprof.so放到php的扩展目录中，并在php.ini中添加extension = xhprof.so 启用扩展

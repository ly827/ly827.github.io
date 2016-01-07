---
title: MAC下tar压缩打包与解压命令详解
title-en: Tar command on MacOS
date: 2014-6-16 14:45:25 +0800
categories: Mac 
tags: mac tar
layout: post
---
### MAC下tar压缩打包与解压命令详解
#### tar命令

	tar -cxtzjvfpPN 文件与目录

常用参数:
-c：建立一个压缩文件（--create）
-x：从文档展开文件（--extract,--get）
-t：列出存档中的文件和目录(--list)
-z：用gzip对存档压缩或解压（--gzip,--ungzip）
-j：是bzip2对存档压缩或解压
-v：详细显示处理的文件（--verbose）
-f：指定存档或设备（--file）
-p：展开所有保护信息（--same-permissions,--preserve-permissions）
-P：不要从文件名中去除‘/’（--absolute-paths）
-N：仅存储时间较新的文件（--after-date DATE,--newer DATE）

#### 范例一：将整个/etc 目录下的文件全部打包成为 /tmp/etc.tar

	tar -cvf /tmp/etc.tar /etc   //仅打包，不压缩
	tar -zcvf /tmp/etc.tar.gz /etc  //打包后用gzip压缩

#### 范例二：查看上面打包的文件/tmp/etc.tar.gz有哪些文件

	tar -ztvf /tmp/etc.tar.gz

由于我们使用了Gzip压缩，所以查看的时也要用上z参数进行解压

#### 范例三：将/tmp/etc.tar.gz文件解压在 /usr/local/src文件夹下

	cd /usr/local/src
	tar -zxvf /tmp/etc.tar.gz






---
title:  "Mac设置zsh为系统shell"
title-en: "Set-Zsh-To-System-Shell-On-MacOS"
date:   2015-12-29 14:45:25 +0800
categories: MacOS
tags: mac zsh shell
layout: post
---
zsh是强大的shell终端 [zsh](#)()
Mac下面已经默认安装了zsh,可以采用如下命令查看

```
`shell
cat /etc/shells
# List of acceptable shells for chpass(1).
# Ftpd will not allow users to connect who are not using
# one of these shells.

/bin/bash
/bin/csh
/bin/ksh
/bin/sh
/bin/tcsh
/bin/zsh
```
`
我们看到系统已经安装zsh，只要将其设为默认shell即可

```
`chsh -s /bin/zsh
```
`
会提示输入密码，输入后重新打开shell就好啦
要下设置会默认shell一样操作：

```
`chsh -s /bin/bash
```
`
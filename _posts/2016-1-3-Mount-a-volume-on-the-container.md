---
title: 为容器配置外部文件映射
title-en: Mount a volume on the container
date:   2016-1-3 14:45:25 +0800
categories: Docker
tags: mac docker
layout: post
---
### 为容器配置外部文件映射

当你需要在启动容器时将你的文件夹 `/Users/username` 映射到VM时，你可以使用容器的共享挂在目录功能，下面我就在做这样的事情。

1. 切换到 `$HOME` 目录
	` $ cd $HOME`
2. 创建新的目录 `site` 并切换至 
	` $ mkdir site && cd site`
3. 创建文件 `index.html`
	` $ echo 'my first site' > index.html`
4. 启动一个新的nginx容器，将`html`目录替换为你的`site`目录
<pre><code>
$ docker run -d -P -v $HMOE/site:/usr/share/nginx/html \
-—name mystic nginx
</code></pre>
5. 获得容器 `mysite` 的端口
	` $ docker port mysite`
6. 在浏览器打开,查看是否显示正常
7. 在站点中添加一个文件 `index-test.html`，实时显示
	` $ echo 'index-test-data' > index-test.html`
8. 停止&删除容器
<pre><code>
$ docker stop mysite 
$ docker rm mysite
</code></pre>

 


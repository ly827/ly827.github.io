---
title: Docker入门
title-en: Docker basis
date:   2016-1-2 14:45:25 +0800
categories: Docker
tags: mac docker
layout: post
---
### Docker入门

#### 什么是docker
> Docker 是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）。几乎没有性能开销,可以很容易地在机器和数据中心中运行。最重要的是,他们不依赖于任何语言、框架包括系统。

#### MacOS安装docker

[ Installation on MacOSX ](https://docs.docker.com/engine/installation/mac/ "Installation on MacOSX")

新版的Docker已经可以使用Docker Toolbox来进行管理Docker了，下面我们进行安装操作；如果你有VirtualBox正在运行，你必须在安装前关闭它们。

1. 下载 [ Docker Toolbox ](# "Docker Toolbox") 

2. 双击安装包或者右击在弹出菜单选择’open‘来安装Docker Toolbox
		![](https://docs.docker.com/engine/installation/images/mac-welcome-page.png "one")
		
3. 点击 `Continue` 安装toolbox
		![](https://docs.docker.com/engine/installation/images/mac-page-two.png "Two")
		默认情况下，标砖Docker Toolbox会安装：
		- 安装Docker tools二进制文件至 `/usr/local/bin`
		- 允许所有用户使用这些二级制文件
		- 安装VirtualBox；或者将现在已经安装的VirtualBox更新为默认定制状态或更改本地安装
		
4. 点击`install` 进行确认安装
		系统会进行确认输入密码
		![](https://docs.docker.com/engine/installation/images/mac-password-prompt.png "Three")
		
5. 输入密码继续进行安装
		When it completes, the installer provides you with some information you can use to complete some common tasks.
		![](https://docs.docker.com/engine/installation/images/mac-page-finished.png "Four")
		
6. 点击`Close` 退出

#### 运行Docker容器

运行Docker容器，你需要做以下工作：
- 创建一个新的Docker虚拟机（或者启动一个已经存在的）
	- 选择你的新VM的环境
	- 用 `docker` 客户端创建、加载、管理容器
	一旦你创建了一个机器，你可以像任何VirtualBox 虚拟机一样复用它。
通常你可以通过Docker Quickstart终端或者从你的Shell启动
1. 从Docker Quickstart终端启
	1. 打开”Application”
	2. 找到Docker Quickstart Terminal 双击启动它
		这个程序会：
			- 打开一个终端窗口
			- 创建一个 `default` VM(如果它不存在)并启动它，
			- 将这终端环境与这个VM关联 
		启动完成后，会显示一个Docker Quickstart Terminal 报告：
			![](https://docs.docker.com/engine/installation/images/mac-success.png)
			现在，你可以运行 `docker` 命令了
	3. 通过运行`hello-world`验证是否安装成功
			`docker run hello-world` 

2. 从Shell终端
	1. 创建新的Docker VM
	
		```
			docker-machine create —-driver virtual box default 
		```
	这个命令将在VirtualBox中创建一个新的VM `default` .同时会创建一个虚拟机配置文件在 `~/.docker/machine/machines/default` 目录，你只需要运行一次 `create` 命令。现在你可以使用 `docker-machine` 命令来启动、停止、查询和其他一些管理VM的命令。
	2. 查看有效的机器
		`docker-machine ls`
	3. 获得你新的VM的环境命令
		`docker-machine env default`
	4. 连接你的shell到 `default` 机器
		`eval "$(docker-machine env dafault)"`
	5. 运行 `hello-world` 容器来校验之前做的步骤是不是正确
		`docker run hello-world`

#### Run Docker on Mac OSX
首先你通过之前的操作，你应该有一个已经运行的VM，并且已经通过终端连接了它，可以使用以下命令来查看是否有效。

`docker-machine ls`

那个`ACTIVE`机器，在这种情况下`default`就是你指向的那个环境

##### 访问容器端口

1. 在DOCKER_DOCKER_HOST中开启一个NGINX容器

	`docker run -d -P --name web nginx`
	
	正常情况下，`docker run` 命令开启一个容器，并运行它， `-d` 可以在退出时保持容器在后台运行，`-P` 选项表示使用容器的公共端口对接本地接口，格式为 `80:80` ，默认为随机端口；这样你就可以从Mac访问他们
	
2. 使用 `docker ps` 命令显示当前正在运行的容器
 
3. 使用命令 `docker port web` 查看刚刚容器的端口映射
	```
		$ docker port web
		443/tcp -\> 0.0.0.0:49156
		80/tcp -\> 0.0.0.0:49157
	```
	这个结果显示 `web` 容器的端口80对应Docker host的端口49157
		
4. 在浏览器中打开地址 http://localhost:49157
	它现在应该还不能工作，它不工作的原因是因为你的`DOCKER_HOST` 地址并不是本地的地址（localhost:0.0.0.0）,而是你安装的Docker VM的地址，而我们刚刚浏览器的地址只是本地的地址
	
5. 获取 `default` VM 的地址
		```
		$ docker-machine ip default
		192.168.59.103
		```
	
6. 在浏览器中输入 `http://192.168.59.103:49157` 即可正确查看

7. 停止、删除正在运行的 `nginx` 容器
		```
		$ docker stop web
		$ docker rm web
		```
		














		 









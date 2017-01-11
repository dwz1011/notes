#Docker

###为什么要使用Docker

**1.更快速的交付与部署**

**2.更高效的虚拟化**

**3.更轻松的迁移和扩展**

**4.更简单的管理**

###基本概念

**镜像(Image)** ----一个只读模板

**容器(Container)** ----运行应用

**仓库(Repository)** ----集中存放镜像文件的场所

###常用命令

**列出本地镜像`docker images`**

	➜  ~ docker images
	REPOSITORY    TAG          IMAGE ID         CREATED          SIZE
	python        2.7          acf0d719f268     2 weeks ago      676.2 MB
	ubuntu        latest       e4415b714b62     7 weeks ago      128.1 MB
	
- REPOSITORY ----来自于哪个仓库
- TAG ----镜像的标记
- IMAGE ID ----ID号(唯一)
- CREATED ----创建时间
- SIZE ----镜像大小

**获取镜像`docker pull`**

	➜  ~ docker pull ubuntu:14.04
	14.04: Pulling from library/ubuntu
	
	16da43b30d89: Pull complete
	1840843dafed: Pull complete
	91246eb75b7d: Pull complete
	7faa681b41d7: Pull complete
	97b84c64d426: Pull complete
	Digest:sha256:881befbe6f54c1e85029fe3a11554342bf765a0849600ecb8fa2f922798b4925
	Status: Downloaded newer image for ubuntu:14.04
	
**使用镜像**

例：创建一个容器，让其中运行bash应用

	➜  ~ docker run -t -i python:2.7 /bin/bash
	root@c94b34a72ec3:/#
	
若不指定具体的标记，则默认使用latest标记信息

####命令
	
	docker version       查看Docker的版本信息	docker build         从一个Dockerfile创建一个镜像	docker history <ID>  显示一个镜像的历史	docker images        列出存在的镜像
	docker info          显示一些相关的系统信息	docker inspect <ID>  显示一个容器的底层具体信息
	docker run           创建一个新容器,并在其中运行给定命令
	docker attach        进入到一个正在运行的容器中
		进入正在运行的容器的方法：
			1.docker attach <ID>
			2.ssh root@<ID>
			3.先拿到进程的PID，然后nsenter --target <PID> --mount --uts --ipc --net --pid
			4.docker exec [options] <ID> /bin/bash
	docker start <ID>    启动一个容器	docker ps            列出容器
		-a, --all            显示所有容器
		-f
		-n
		-l, --latest         显示最近一个使用的容器
		-q
		-s
	docker stop          终止一个运行中的容器
	docker restart       重启一个运行中的容器
	docker kill <ID>     删除一个处于终止状态中的容器
	￼docker rm <ID>       删除一个(或多个)处于终止状态中的容器
		-f                    删除一个(或多个)运行中的容器
	docker rmi           删除给定的一个(或多个)个镜像
	docker top <ID>      查看一个容器中的正在运行的进程信息
	docker logs <NAMES>  获取容器的log信息
	
	
	docker import        导入一个文件(典型为tar包)路径戒目录来创建一个镜像
	docker commit        从一个容器的修改中创建一个新的镜像	docker cp            从容器中复制文件到宿主系统中	docker diff          检查一个容器文件系统的修改	docker events        从服务端获取实时的事件	docker export        导出容器内容为一个tar包
	docker load          从一个tar包中加载一个镜像	docker login         注册戒登录到一个Docker的仏库服务器	docker logout        从Docker的仓库服务器登出	docker pause         暂停一个容器中的所有迕程	docker port          查找一个nat到一个私有网口的公共口	docker pull          从一个Docker的仓库服务器下拉一个镜像或仓库	docker push          将一个镜像或者仓库推送到一个Docker的注册服务器	docker save          保存一个镜像为tar包文件	docker search        在Docker index中搜索一个镜像	docker tag           为一个镜像打标签	docker unpause       将一个容器内所有的迕程从暂停状态中恢复	docker wait          阻塞直到一个容器终止,然后输出它的退出符
	
	
####利用Dockerfile来创建镜像

**`docker build`**

首先，需要创建一个Dockerfile，包含一些如何创建镜像的指令
	
	新建一个目录和一个Dockerfile
	➜  ~ mkdir sinatra
	➜  ~ cd sinatra
	➜  sinatra touch Dockerfile
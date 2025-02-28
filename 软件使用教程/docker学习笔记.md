# docker
## docker起源
### 发布时间
2013年3月 DotCloud公司创始人Solomon Hykes 

### 背景
1. 传统虚拟机占用资源多,启动慢,隔离效果差等等
2. 软件开发环境配置麻烦
### linux容器
linux容器不是一个完整的操作系统,而是对进程进行隔离,或者是对进程添加保护层,对于内部的进程所接触的资源都是虚拟化的,实现与底层系统隔离.优点是:启动快,占用资源少,体积小.  
docker属于linux容器的一种封装,提供简单易用的容器使用接口.

### docker的用途
* 简化应用程序的部署和管理
* 提供弹性的云服务

## docker安装

### 官网安装教程:

* [mac](https://docs.docker.com/desktop/install/mac-install/)
* [windows](https://docs.docker.com/desktop/install/windows-install/)
* [linux](https://docs.docker.com/desktop/install/linux-install/)
### 检验安装成功
```
docker version
或
docker info
```
### docker操作
#### 启动docker
```
service docker start
或
systemctl start docker
```
#### 重启docker
```
systemctl restart docker
```
#### 关闭docker
```
systemctl stop docker
```
## docker之image
### 简介

* image中必须包含应用程序运行所包含的一切.
* image和container的关系:image可以看作容器的模板,而容器可以根据image生成多个容器实例(类似于类与对象的关系)
### 基本命令
1. 查看所有image
```
docker images
或者
docker image ls
```
2. 删除image文件
```
docker rmi <image-id>
或者
docker image rm <image-id>
```
3. 搜索镜像
```
docker search image名
```
### image仓库
1. docker官方仓库:[docker hub](https://hub.docker.com/)
2. docker image和docker仓库的关系:image可以上传到网上的仓库中,本地仓库存放本地的image 
### 从docker hub拉取image

```
docker image pull library/hello-world
```
`docker image pull`是抓取image的命令.`library/hello-world`是image在仓库位置,library是官方默认组,官方提供的image都存放在`library`中

### 运行image
```
docker run hello-world
```
`docker container run`会生成一个运行的容器实例,且该命令可以自动抓取image,无需`docker image pull`

## docker之container

### 简介
image生成的容器,本身也是一个文件,称为容器文件.容器运行结束后不会删除.
### 指令

1. 查看容器
```
正在运行的容器
docker ps
所有的容器
docker ps -a
```
2. 停止容器运行
```
# 立刻终止
docker kill <container-id>
# 执行收尾工作且可能会不理会停止工作
docker stop <container-id>
```
3. 删除容器
```
docker rm <container-id>
```
4. 退出容器
```
Ctrl+d (或exit)
```
5. 运行image(创建container)
```
docker run <image-name>
```
6.运行容器,结束后删除容器
```
docker run --rm -p <端口映射> -it <shell指令映射>  <image-name>
```
### Dockerfile文件中Run和CMD命令
1. `CMD`命令表示:容器启动后自动执行的指令
2. `RUN`命令在构建image文件的阶段执行,执行结果都会打包进入image文件中
3. `CMD`命令则是在容器启动后执行
4. 一个 Dockerfile 可以包含多个`RUN`命令，但是只能有一个`CMD`命令
## 制作Docker
### 准备源码
### 编写Dockerfile文件
1. 新建.dockerignore
```
排除路径(相对路径),表示不打包进入image文件
```
2. 新建Dockerfile文件
```
# 基础镜像
FROM ubuntu:latest
# 拷贝文件到容器中
COPY . /app
# 安装软件包
RUN apt-get update && apt-get install -y \
    python3 \
    python3-pip
# 设置工作目录
WORKDIR /app
# 运行命令
CMD ["python3", "app.py"]

```
### 创建image文件
```
docker image build -t kao-demo:版本信息 .
```
### 生成容器
```
docker run --rm -p<端口映射> -it<shell命令映射> 
```
### 发布image
1. 登录docker hub
```
docker login
```
2. image标注用户名和版本
```
docker image tag [imageName] [username]/[repository]:[tag]
```
3. 发布image
```
docker image push [username]/[repository]:[tag]
```
## docker微服务教程

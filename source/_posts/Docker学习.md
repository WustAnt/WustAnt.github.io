# Docker基础知识

> 推荐学习时看的一些大佬的教程，挺详细的。
>
> [Docker-2020最新超详细版教程通俗易懂](https://www.lixian.fun/3812.html)

## 一、Docker  安装

### 1.下载docker的依赖环境

```sh
yum -y install yum-utils device-mapper-persistent-data lvm2
```

### 2.指定Docker的镜像

```sh
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

### 3.安装Docker

```sh
yum makecache fast
yum -y install docker-ce
```

### 4.启动Docker并测试

```sh
安装成功后，需要手动启动，设置为开机启动，并测试一下 Docker
#启动docker服务
systemctl start docker
#查看docker状态
systemctl status docker
#停止docker服务
systemctl stop docker
#设置开机自动启动
systemctl enable docker
#测试
docker run hello-world
```

## 二、Docker的中央仓库

```sh
1.Docker官方的中央仓库：这个仓库是镜像最全的，但是下载速度较慢。
https://hub.docker.com/
2.国内的镜像网站：网易蜂巢，daoCloud等，下载速度快，但是镜像相对不全。
https://c.163yun.com/hub#/home 
http://hub.daocloud.io/ （推荐使用）
3.在公司内部会采用私服的方式拉取镜像（添加配置）
#需要创建 /etc/docker/daemon.json，并添加如下内容
{
    "registry-mirrors":["https://registry.docker-cn.com"],
    "insecure-registries":["ip:port"]
}
#重启两个服务
systemctl daemon-reload
systemctl restart docker
```

## 三、镜像操作

### 1.拉取镜像

```sh
#从中央仓库拉取镜像到本地
docker pull 镜像名称[:tag]
#举个例子:docker pull daocloud.io/library/tomcat:8.5.16-jre8-alpine  
```

### 2.查看本地镜像

```sh
#查看本地已经安装过的镜像信息，包含标识，名称，版本，更新时间，大小
docker images
```

### 3.删除本地镜像

```sh
#镜像会占用磁盘空间，可以直接手动删除，标识通过查看获取
docker rmi 镜像的标识
```

### 4.镜像的导入导出

```
#将本地的镜像导出
docker save -o 
导出的路径 镜像id
#加载本地的镜像文件
docker load -i 镜像文件
#修改镜像文件
docker tag 镜像id 新镜像名称：版本
```

## 四、容器操作

### 1.运行容器

```sh
运行容器需要定制具体镜像，如果镜像不存在，会直接下载
#简单操作
docker run 镜像的标识|镜像的名称[:tag]
#常用的参数
docker run -d -p 宿主机端口:容器端口 --name 容器名称 镜像的标识|镜像名称[:tag]
#-i:表示运行容器
#-t:表示容器启动后会进入其命令行。加-it参数后，容器创建后直接进入到容器中，输入exit退出容器
#-d:代表后台运行容器
#-p 宿主机端口:容器端口：为了映射当前Linux的端口和容器的端口
#--name 容器名称:指定容器的名称

```

### 2.查看正在运行的容器

```sh
docker ps [-qa]
#-a：查看全部的容器，包括没有运行
#-q：只查看容器的标识
#-l：
#-f status=exit：查看停止的容器
```

### 3.查看容器的日志

```sh
docker logs -f  容器id
#-f：可以滚动查看日志的最后几行
```

### 4.进入容器的内部

```sh
#可以进入容器的内部进行操作
docker exec -it 容器id bash
```

### 5.复制内容到容器

```sh
#将宿主机的文件复制到容器内部的指定目录
docker cp 文件名称 容器id:容器内部路径
```

### 6.重启&启动&停止&删除

```sh
容器的启动，停止，删除等操作，后续会经常使用到
#重新启动容器
docker restart 容器id
#启动停止运行的容器
docker start 容器id

#停止指定的容器(删除容器前，需要先停止容器)
docker stop 容器id
#停止全部容器
docker stop $(docker ps -qa)
#删除指定容器
docker rm 容器id
#删除全部容器
docker rm $(docker ps -qa)
```



------

> 声明，本文非教程，小白自学时简单梳理的内容，有不妥的地方欢迎指正。




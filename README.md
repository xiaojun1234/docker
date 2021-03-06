# docker
## 简单命令
下载docker:`yum install -y docker` \
查找容器：`docker search` \
创建容器：`docker run -itd --name jun ubuntu /bin/bash` \
  -i:标准输入；-t：tty控制台；-d：是后台运行 \
再次打开容器：`docker exec -itd jun /bin/bash` 或 `docker attach jun` \
网上拉取镜像：`docker pull ubuntu:14.04` \
查看镜像：`docker images` \
查看运行容器：`docker ps -al` \
开启容器：`docker start jun` \
停止容器：`docker stop jun` \
删除容器：`docker rm jun` \
删除镜像：`docker rmi jun01` \
删除所有容器：`docker rm docker ps -aq` \
删除所有镜像：`docker rm docker images -q`
## 镜像构建
直接生成镜像并改名： \
`docker commit` \
`--author "jun <jun@images.com>"` \
`--message "修改了默认网页"` \
`jun01` \
`jun01:v2` \
使用git repo构建： \
`docker build https://github.com/twang2218/gitlab-ce-zh.git#:8.14` \
`docker build https://github.com/twang2218/gitlab-ce-zh.git\#8.14` \
使用tar压缩包构建： \
`docker build http://server/context.tar.gz` 
### 从标准输入中读取Dockerfile进行构建:
`docker build - < Dockerfile` \
`docker build | docker build -`
### 从标准输入中读取上下文压缩包进行构建:
`docker build - < context.tar.gz`
###  镜像配置文件
`vim Dockerfile` \
基础镜像：`FROM nginx` \
虚悬镜像：即空镜像 \
容器里跑命令：`RUN echo "<h1>this is docker<h2>" > /usr/share/nginx/html/index.html` \
复制和高级复制：COPY 和 ADD "都是在上下文包（context）中进行的" \
`COPY pack.json /usr/src/app/` \
ADD源路径可以是URL，多用于自动解压的复制， \
`ADD ubuntu-xenial-core-cloudimg-amd64-root.tar.gz /` 
### 执行Dockerfile
执行构建：`docker build -t web .` \ 
查看过程：`docker history web`
### 端口和挂载卷
绑定容器端口并挂载卷： \
`docker run -d -p 80 --name jun01 jun nginx -v $PWD/website:/var/www/html/website --privileged=true ` \
设置挂载文件权限(rw或ro)： \
`docker run -d -p 80 --name jun01 jun nginx -v $PWD/website:/var/www/html/website：rw/ro --privileged=true ` \
设置简单本地映射：`docker run -d -p 8080:80 --name jun01 jun nginx`
### CMD用法
shell 格式：CMD <命令> \
exec 格式：CMD ["可执行文件","参数1","参数2"] \
解析之后主进程是“sh”,不是“echo $HOME”, \
`CMD echo $HOME` 相当于 `CMD ["sh","-c","echo $HOME"]` \
启动nginx服务：CMD["nginx","-g","daemon off"] \
### entrypoint入口点
用法和CMD差不多,不过比CMD灵活,它可以外接参数 \
Dockerfile:`ENTRYPOINT ["curl","-s","http://ip.cn"]` \
`docker run jun0 -i` \
还有一个用法：用于用户服务权限问题 \
`Dockerfile:` \
`FROM alpine:3.4` \
`RUN addgroup -S redis && adduser -S -G redis redia` \
`ENTRYPOINT ["docker-entry.sh"]` \
`EXPOSE 6379` \
`CMD ["redis-server"]` \
创建脚本判断是否是redis用户：`vim docker-entrypoint.sh` \
`#!/bin/sh` \
`if ["$1"='redis-server' -a "$(id -u)" = '0' ]; then` \
`chown -R redis .` \
`exec su-exec redis "$0" "$@"` \
`fi` \
`exec "$@"`
### 设置环境变量
在Dockerfile设置环境变量,方便以后更新改变 \
`ENV VERSION=1.0 DEBUG=on NAME="Happy Feet"` \
`ARG`用法与`ENV`一样,只是在将来容器运行时不会存在这些变量,最好不要用来设置密码
### 定义匿名卷
`VOLUME /data` \
运行时也可覆盖：`docker run -d -v jun0:/data xxxx` \
### 指定工作目录
WORKDIR可以改变以后每层的路径 \
`WORKDIR /website(工作路径）`
### 指定当前用户
`USER <用户名>` \
切换用户最好用`gosu`,出错率低,快,配置如下：
`RUN groupadd -r redis && useradd -r -g redis redis` \
`RUN wget -O /usr/local/bin/gosu "https；//github.com/tianon/gosu/releases/download/1.7/gosu-amd64" \` \
`&& chmod +x /usr/local/bin/gosu \` \
`&& gosu nobody true` \
`CMD ["exec","gosu","redis","redis-server"]` \





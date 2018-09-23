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
删除所有镜像：`docker rm docker images -q` \
## 镜像构建Dockerfile
直接生成镜像并改名：`docker commit \
--author "jun <jun@images.com>" \
--message "修改了默认网页" \
jun01 \ 
jun01:v2` \
镜像配置文件：Dockerfile
### vim Dockerfile 
基础镜像：`FROM nginx` \
虚悬
容器里跑命令：`RUN echo "<h1>this is docker<h2>" > /usr/share/nginx/html/index.html` \
### 执行Dockerfile
执行构建：`docker build -t web .` \ 
查看过程：`docker history web` \
绑定容器端口并挂载卷：`docker run -d -p 80 --name jun01 jun nginx -v $PWD/website:/var/www/html/website --privileged=true ` \
设置挂载文件权限(rw或ro)：`docker run -d -p 80 --name jun01 jun nginx -v $PWD/website:/var/www/html/website：rw/ro --privileged=true ` \
设置简单本地映射：`docker run -d -p 8080:80 --name jun01 jun nginx` \



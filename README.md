# docker
## 简单命令
下载docker:`yum install -y docker`
查找容器：`docker search`
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
镜像配置文件：Dockerfile
### Dockerfile 

绑定容器端口：`docker run -d -p 80 --name jun01 jun nginx -v $PWD/website:/var/www/html/website` \

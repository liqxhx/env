[ref](http://blog.csdn.net/zhouhuakang/article/details/51098066)
## 安装vritubox
[下载](http://www.oracle.com/technetwork/server-storage/virtualbox/downloads/index.html)
## 安装vagrant
下载系统对应版本一路下一步即可安装成功，用`vagrant -v`查看版本信息  
[下载](https://www.vagrantup.com/downloads.html)  
[使用](https://www.vagrantup.com/docs/cli)

```
pwd
/Users/liqh

#Vagrant Parallels provider is compatible only with Pro and Business editions of Parallels Desktop.
#vagrant plugin install vagrant-parallels
mkdir -p my-vagrant-project/ubuntu14.04
cd my-vagrant-project/ubuntu14.04

vagrant init bento/ubuntu-14.04 --box-version 201708.22.0

vagrant up
#下载等待
#vagrant up --provider hyperv
vagrant ssh


```
常用命令<br />
vagrant package --output ubuntu1404.box<br />
vagrant suspend 挂起<br />
vagrant reload 重启<br />

vagrant box list <br />
vagrant box add name url<br />
vagrant init name<br />

## 配置vagrant
sudo useradd -d /home/liqh -m -s /bin/zsh
```
vi Vagrantfile
config.vm.box_check_update = false
config.vm.network "public_network", use_dhcp_assigned_default_route: true
```


## 在虚拟机中安装docker
[docker](https://www.docker.com/)  <br />
linux中可以用下面方式安装
```
uname -r #检察内核版本 > 3.
sudo wget -qO- https://get.docker.com/ | sh #要fq
# sudo curl -s https://get.docker.com | sh
sudo usermod -aG docker liqh #加入docker用户组
 
#--------基本使用-----------
docker imsages # 查看本地镜像
docker pull hub.c.163.com/library/nginx:latest # 拉取远程镜像到本地
docker images 
docker ps
docker run --help
docker run -d -p 8080:80 hub.c.163.com/library/nginx #运行容器并指定端口映射，本地:容器
netstat -nap|grep 8080
#docker run -d -P hub.c.163.com/library/nginx 随机分配端口
docker ps
docker exec -it a047 bash # 进入容器内部
docker stop a047 # 停止指定容器
docker ps
-----------------------
# 制作自己的镜像
docker images
docker pull hub.c.163.com/library/tomcat:latest #拉取tomcat
docker pull hub.c.163.com/library/mysql:latest #拉mysql
docker images
docker ps
docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -e MYSQL_DATABASE=jpress hub.c.163.com/library/mysql #运行mysql,创库，指定root密码
# http://jpress.io
# https://github.com/JpressProjects/jpress/tree/alpha/wars
# wget https://github.com/JpressProjects/jpress/blob/alpha/wars/jpress-web-newest.war
vi Dockerfile
from hub.c.163.com/library/tomcat
MAINTAINER 作者信息
COPY jpress-web-newest.war /usr/local/tomcat/webapps

docker build --help
docker build . -t jpress:latest # 构建镜像
docker images
dcoker run -d -p 8800:8080 jpress # 运行自己的容器

# 自制镜像
docker run -p 8880:80 -d hub.c.163.com/library/nginx # 
docker cp index.html ab5f://usr/share/nginx/html # 添加内容
docker commit -m"add my index.html" ab5 nginx-fun  # 提交 保存 nginx-fun为新images的名字
docker rmi image_id # 根据imageId删除image
docker ps -a # 显示所有已运行的容器历史
docker rm container_id1 container_id2 # 删除容器
```

## Dockerfile
FROM base image
RUN 在容器内执行命令
ADD 往容器内添加文件，比COPY强大点，可以添加文件、目录、远程文件
COPY 往容器内拷贝文件或目录
CMD 指定容器执行程序入口
ENTRYPOINT 与CMD一样，只是格式不一样，如果没有指定ENTRYPOINT则用CMD来启动，指定了ENTRYPOINT则用entrypoint，CMD所指示的内容将会作为entrypoint的参数
WORKDIR 指定程序运行的路径
ENV 为容器里设定环境变量
MAINTAINER 指定容器作者、邮箱
USER 指定执行命令的用户，一般不会在容器中用root来执行命令
VOLUME mount point
EXPOSE 暴露端口

```
echo "hello" > index.html

docker pull ubuntu
docker pull alpine  #alpine：是针对docker制作的一个极小linux环境

vi Dockerfile
FROM alpine:latest
MAINTAINER liqh
CMD echo 'hello docker'

docker build -t alpine-hello-docker .
docker images
docker ps
docker run alpine-hello-docker:latest 

vi Dockerfile
FROM ubuntu
MAINTAINER liqh
RUN sed -i 's/archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
RUN apt-get update
RUN apt-get install -y nginx
COPY index.html /var/www/html
ENTRYPOINT ["/usr/sbin/nginx","-g","daemon off;"]
EXPOSE 80

docker build -t nginx_in_ubuntu .

```


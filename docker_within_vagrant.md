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

```

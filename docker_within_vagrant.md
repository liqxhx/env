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
一般先vagrant box add xxx xxx.box添加一个box到vagrant系统
再创建一个工作目录，当做虚拟机的工作环境目前（可以认为该目录就是一个虚拟机）
cd到工作目录，执行vagrant init进行初始化，会生成一个VagrantFile。
通过该文件可以进行一个网络、同步文件夹、ssh端口的配置

#Vagrant Parallels provider is compatible only with Pro and Business editions of Parallels Desktop.
#vagrant plugin install vagrant-parallels
vagrant box list 
vagrant box add name url/local_path_filename.box 
mkdir -p my-vagrant-project/ubuntu14.04
cd my-vagrant-project/ubuntu14.04

vagrant init bento/ubuntu-14.04 --box-version 201708.22.0

vagrant up
#下载等待
#vagrant up --provider hyperv
vagrant ssh



常用命令
打包
打包前先vagrant halt停止虚拟机，如果不带--output将默认在当前路径生成package.box的文件，--output可自定义打包后的文件名称
vagrant package --output ubuntu1404.box 




vagrant suspend 挂起 
vagrant reload 重启 


vagrant init name 
```
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
docker ps -a
docker stop a047 && docker rm -v a047
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
FROM base image<br />
RUN 在容器内执行命令<br />
ADD 往容器内添加文件，比COPY强大点，可以添加文件、目录、远程文件<br />
COPY 往容器内拷贝文件或目录<br />
CMD 指定容器执行程序入口<br />
ENTRYPOINT 与CMD一样，只是格式不一样，如果没有指定ENTRYPOINT则用CMD来启动，指定了ENTRYPOINT则用entrypoint，CMD所指示的内容将会作为entrypoint的参数<br />
WORKDIR 指定程序运行的路径<br />
ENV 为容器里设定环境变量<br />
MAINTAINER 指定容器作者、邮箱<br />
USER 指定执行命令的用户，一般不会在容器中用root来执行命令<br />
VOLUME mount point<br />
EXPOSE 暴露端口<br />

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
## [volumes](https://docs.docker.com/engine/admin/volumes/volumes/)

## registry
1 
```
docker login

#docker tag local-image:tag repo:tag
#docker pull repo:tag


```
2 [私服](https://docs.docker.com/registry/)
```
docker pull registry:2
docker images
docker run -d -p 5000:5000 registry:2 
#http://docker-on-mac:5000/v2/_catalog
#curl http://docker-on-mac:5000/v2/_catalog
#curl http://docker-on-mac:5000/v2/alp/tags/list
docker tag alpine:3.7 docker-on-mac:5000/alpine:3.7
docker push docker-on-mac:5000/alpine:3.7

#repo
#mac
# 192.168.1.4 docker-on-mac
#vi ~.docker/daemon.js
{
  "debug" : true,
  "experimental" : true,
 "insecure-registries":["docker-on-mac:5000"]
}

#client
#ubuntu
#192.168.1.5
#su - root
#vi /etc/docker/daemon.js
#{"insecure-registries":["docker-on-mac:5000"]}
#docker pull docker-on-mac:5000/alpine:3.7
```

## [alpine](https://www.cnblogs.com/zhangmingcheng/p/7122386.html)

## FAQ
1 在MacOS下，Docker　images保存在哪个路径下 [ref](http://blog.csdn.net/tony1130/article/details/53181071)
/Users/{YourUserName}/Library/Containers/com.docker.docker/Data/com.docker.driver.amd64-linux/Docker.qcow2

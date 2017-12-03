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
docker pull hub.c.163.com/library/nginx:latest
```

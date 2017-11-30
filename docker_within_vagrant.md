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
#下载等待
vagrant up
#vagrant up --provider hyperv
vagrant ssh
```
## 在虚拟机中安装docker
[docker](https://www.docker.com/)

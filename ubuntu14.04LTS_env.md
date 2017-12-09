## 换源
[ref](ubuntu_%E6%8D%A2%E6%BA%90.md)
```
sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup
sed -i 's/archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
sudo apt-get update
```
## 五笔
```
sudo apt-get install fcitx-table-wbpy
配置fcitx：键盘输入方式选fcitx，应用到整个系统
重启电脑
```
## git
[ref](https://git-scm.com/download/linux)

```
add-apt-repository ppa:git-core/ppa
apt update
apt install git
```


## oh my zsh
[ref](ubuntu_ohmhsh.md)

## sdkman
[ref](http://sdkman.io/install.html)<br />
java<br />
maven<br />
gradle<br />
scala<br />
sbt<br />
groovy<br />

## intellij idea
[IntelliJ IDEA](http://www.jetbrains.com/idea/)

## virtubox
[Linux_Downloads](https://www.virtualbox.org/wiki/Linux_Downloads)

## wine

## shadowsocks
[下载](https://github.com/shadowsocks/shadowsocks/releases)
```
export PYTHONPATH=$PYTHONPATH:~/shadowsocks:~/shadowsocks/lib/python2.7/site-packages/
mkdir -p ~/shadowsocks/lib/python2.7/site-packages/
解压下载的shadowsocks
python setup.py build
python setup.py install --prefix=~/shadowsocks
如果失败，根据提示相应处理。成功后
cd ~/shadowsocks/bin
./sslocal -s serverIp -p serverPort -k "password" -l localPort -t timeOut -m cypherMethod

google-chrome --proxy-server=socks5://127.0.0.1:1080

./sslocal --help 
usage: sslocal [OPTION]...
A fast tunnel proxy that helps you bypass firewalls.

You can supply configurations via either config file or command line arguments.

Proxy options:
  -c CONFIG              path to config file
  -s SERVER_ADDR         server address
  -p SERVER_PORT         server port, default: 8388
  -b LOCAL_ADDR          local binding address, default: 127.0.0.1
  -l LOCAL_PORT          local port, default: 1080
  -k PASSWORD            password
  -m METHOD              encryption method, default: aes-256-cfb
  -t TIMEOUT             timeout in seconds, default: 300
  -a ONE_TIME_AUTH       one time auth
  --fast-open            use TCP_FASTOPEN, requires Linux 3.7+

General options:
  -h, --help             show this help message and exit
  -d start/stop/restart  daemon mode
  --pid-file PID_FILE    pid file for daemon mode
  --log-file LOG_FILE    log file for daemon mode
  --user USER            username to run as
  -v, -vv                verbose mode
  -q, -qq                quiet mode, only show warnings/errors
  --version              show version information

Online help: <https://github.com/shadowsocks/shadowsocks>

#####   -c ~/ss.json
{
"server":"ip",
"server_port":port,
"local_port":1080,
"password":"xxx",
"timeout":600,
"method":"aes-256-cfb",
"fast_open": true,
"workers": 1
}

```
[ubuntu图形界面shadowsocks-qt5](https://github.com/shadowsocks/shadowsocks-qt5)
## oracle instantclient

## mycli
wget https://bootstrap.pypa.io/get-pip.py  --no-check-certificate <br />
sudo python get-pip.py <br />
sudo pip install mycli <br />
## docker
[ref](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#install-docker-ce)<br />
1 [set up Docker’s repositories](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#install-using-the-repository) <br />
* SET UP THE REPOSITORY<br />
* INSTALL DOCKER CE<br />

2 [Install it manually](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#install-from-a-package) <br />

3 [convenience scripts](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#upgrade-docker-ce-1) <br />
[docker-install](https://github.com/docker/docker-install)


## 其它
[Ubuntu在标题栏显示CPU、内存、网速和温度](http://blog.csdn.net/u013541140/article/details/50629825)
[Ubuntu 14.04 标题栏实时显示上下行网速、CPU及内存状态](http://blog.csdn.net/Oct11/article/details/44783443)

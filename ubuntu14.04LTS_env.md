## 换源
[ref](ubuntu_%E6%8D%A2%E6%BA%90.md)
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
[ref](http://sdkman.io/install.html)
java<br />
maven<br />
gradle<br />
scala<br />
sbt<br />
groovy<br />
产
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


```
[ubuntu图形界面shadowsocks-qt5](https://github.com/shadowsocks/shadowsocks-qt5)
## oracle instantclient

## mycli


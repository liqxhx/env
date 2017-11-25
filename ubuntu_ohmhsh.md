## 安装zsh
```
# 查看是否安装zsh
echo /etc/shells 
# 如果没有安装先安装zsh
# sudo apt install zsh
#或sudo apt-get install zsh
```

## 安装oh my zsh
[官网](http://ohmyz.sh/)  

安装命令："$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`

## 插件
[zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)<br />
[zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)<br />
[]()<br />

## lambda-mod主题
![](https://xiaozhou.net/pics/iterm/4.png)<br />
[lambda-mod.zsh-theme](https://github.com/TimothyYe/mydotfiles/blob/master/lambda-mod.zsh-theme)

## agnoster主题
1. [安装Powerline对应的字体库](https://github.com/powerline/fonts)  
在终端中选Powerline字体：`12pt Meslo LG S DZ Regular for Powerline`  <br />
编辑 >> 配置文件... >> 复制default,命名为my >> 选择启动时使用的配置文件为my >> 编辑my >> 常规 >> 字体 >> 选择字体：Meslo LG S DZ Regular for Powerline
  <br />

然后执行(一定要在item2中先选择power的字体)> echo "\ue0b0 \u00b1 \ue0a0 \u27a6 \u2718 \u26a1 \u2699"  
显示这样说明字体没有问题
![](https://gist.githubusercontent.com/agnoster/3712874/raw/characters.png)譓

2. 生效配置
```
vi ~/.zshrc
#ZSH_THEME="robbyrussell"
#ZSH_THEME="lambda-mod"
ZSH_THEME="agnoster"

source ~/.zshrc
```

# Mac折腾记

## 一些好用的功能
[触控板用力点按 Froce torch](https://www.macdaxue.com/force-touch/)

## 系统相关

### 修改账户名称
在账户相关设置中，有账户名称和全名两个设置。如果想要修改账户名称，需要新建一个管理员账户，然后退出当前账户，在另一个管理员账户中修改账户名称。如果想修改账户全名，需要重置用户文件夹。

### 环境变量

[Mac在bash和zsh配置环境变量的几种方法](https://www.jianshu.com/p/020f3d02f538)

这里需要注意的是在配置用户环境变量时，`~/.bashrc`对应bash的用户环境变量。如果已经把bash改成了zsh，需要在`~/.zshrc`中设置。

### bash->iterms2

[iTerm2 + Oh My Zsh 打造舒适终端体验](https://www.jianshu.com/p/9c3439cc3bdb)

我的一些配置：

```text
iterms Theme：Dracula
font: 18pt Roboto Mono for Powerline
Hotkey: Double click Command
```

### 替换`Command`和`Ctrl`
系统偏好设置->键盘->修饰键

### Finder标题显示文件夹全称
`defaults write com.apple.finder _FXShowPosixPathInTitle -bool YES`

## 常用软件

### MAC软件网站
[xxmac](https://www.xxmac.com/)

[xclient](https://xclient.info/)

[MacBL](https://www.macbl.com/)

### 软件卸载工具
[好用的软件卸载工具](https://segmentfault.com/a/1190000005035742)

### Pycharm
[安装Pycharm及破解](https://zhile.io/2018/08/17/jetbrains-license-server-crack.html)

### Anaconda
[Anaconda官网](https://www.anaconda.com/distribution/)

将Anaconda加入环境变量：在`~/.bashrc`或`~./zshrc`中加入`export PATH="/Users/binbin/anaconda3/bin:$PATH"`

### 多电脑共享键鼠Synergy
> [度娘网盘](https://pan.baidu.com/s/1PV_aY2BdgqmeFI5vLsEBXA)
>
> 提取码：sk6h

需要注意的：

1. 在第一次使用时需要去掉`编辑->设置->Use SSL Encryption`的勾，然后删除`~/Library/Synergy/SSL`文件夹

2. 要写清楚主机和从机在局域网中的名称

3. 如果从机是Mac，可以把Super和Ctrl键进行替换

4. 如果出现不能复制粘贴的情况，可以重启Synergy

5. 如果出现已连接但不能移动鼠标的情况，可以在主机端把从机清除再添加一次


### Mactex
[MacTex](http://www.tug.org/mactex/)

### Dash
[安装](https://kapeli.com/dash)
[破解](https://kakarot.net/cgi-bin/dash-macos-license)
[DocSet下载](https://github.com/Kapeli/feeds)。下载后存放到`/Users/name/Library/Application Support/Dash/`中对应位置

### 网速显示
[MenuMesters](https://github.com/yujitach/MenuMeters)

### 在目录下打开shell
[go2Shell](https://zipzapmac.com/Go2Shell)

### 视频播放器
[IINA](https://iina.io/)
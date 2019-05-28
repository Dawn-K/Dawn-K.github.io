---
layout:     post
title:      "manjaro折腾小记"
subtitle:   "Just For Fun"
date:       2019-05-28 12:33:00
author:     "DawnK"
header-img: "img/post-bg-2015.jpg"
tags:
    - Linux
---

# manjaro折腾小记

使用manjaro半年了,中途遇见很多很多问题,在此记录,以备以后查询之用

## 参考资料

[GAWEGOR](https://www.jianshu.com/p/4fce765a306b)

[imorta](https://blog.csdn.net/weixin_41301508/article/details/81193217)

## 源设置

### 设置官方镜像源(core,extra,community,multilib)

```bash
sudo pacman-mirrors -i -c China -m rank //更新镜像排名,图形化界面,选中前几个即可
sudo pacman -Syy //更新数据源
```

### 设置archlinuxcn源

修改 /etc/pacman.conf　　=> 末尾添加

```
[archlinuxcn]
SigLevel = Optional TrustedOnly
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

## 目录设置

### 修改Home下的目录为英文

1. 修改目录映射文件名；

```bash
vim .config/user-dirs.dirs
```

修改为以下内容：

```
XDG_DESKTOP_DIR="$HOME/Desktop"
XDG_DOWNLOAD_DIR="$HOME/Download"
XDG_TEMPLATES_DIR="$HOME/Templates"
XDG_PUBLICSHARE_DIR="$HOME/Public"
XDG_DOCUMENTS_DIR="$HOME/Documents"
XDG_MUSIC_DIR="$HOME/Music"
XDG_PICTURES_DIR="$HOME/Pictures"
XDG_VIDEOS_DIR="$HOME/Videos"
```

1. 将Home目录下的中文目录名改为对应的中文名；
2. 重启系统。

## 软件操作

```安装 pacman -S ```
```删除 pacman -R ```
```移除已安装不需要软件包 pacman -Rs ```
```删除一个包,所有依赖 pacman -Rsc ```
```升级包 pacman -Syu ```
```查询包数据库 pacman -Ss ```
```搜索以安装的包 pacman -Qs ```
```显示包大量信息 pacman -Si ```
```本地安装包 pacman -Qi ```
```清理包缓存 pacman -Sc ```

## 显卡驱动

[taoist](https://mtaoist.xyz/2018/03/19/Bumblebee/)

### 安装bumblebee

Manjaro 提供了强大的硬件检测模块`mhwd`,可以很方便的安装各种驱动。

- 安装依赖

  > sudo pacman -S virtualgl lib32-virtualgl lib32-primus primus

- 安装nvidia闭源驱动与intel驱动混合版bumblebee

  > sudo mhwd -f -i pci video-hybrid-intel-nvidia-bumblebee

- 开启自动启动bumblebeed服务

  > sudo systemctl enable bumblebeed

- 将用户添加到bumblee组

  > sudo gpasswd -a $USER bumblebee

如果一切顺利的话，**重启后就可以在你想运行的程序名前面加`optirun`,好使用独立显卡驱动你的应用程序**。如果出现启动无法进入图形界面的问题,本笔记中也有解决方法.

### 测试性能

- 安装测试软件

  > sudo pacman -S mesa-demos

- 集显性能

  > glxgears -info

- 独显性能

  > optirun glxgears -info

可以看到独显带来的提升非常巨大,大概是集成显卡帧数30多倍

## 输入法

我们安装基于fcitx的搜狗输入法

```bash
sudo pacman -S fcitx-sogoupinyin
sudo pacman -S fcitx-im         # 全部安装
sudo pacman -S fcitx-configtool # 图形化配置工具
```

设置中文输入法环境变量，编辑~/.xprofile文件，增加下面几行(如果文件不存在，则新建)

```
exportGTK_IM_MODULE=fcitx
exportQT_IM_MODULE=fcitx
exportXMODIFIERS="@im=fcitx"
```

保存成功后，在终端输入fcitx启动服务后，会增加一个键盘的托盘图标，右击这个图标，打开配置工具，在输入法栏目中增加sogoupinyin输入法。

重启后就能正常使用了。

偶尔输入法崩溃,卸载重装就好了,这个还比较良心.

## WPS

wps算是比较良心的了,目前是最好的office替代品,

```bash
sudo pacman -S wps-office
sudo pacman -S ttf-wps-fonts
```

但是!在显示方面可能会有一些问题,比如在白色底下的字体可能是灰色,导致看不清,所以在使用wps前可以把系统的主题设置成白色的,这样就解决了此问题.

## 开机卡死问题

编辑 /etc/default/grub,前几行改成这样

```
GRUB_DEFAULT=saved
GRUB_TIMEOUT=5
GRUB_DISTRIBUTOR='Manjaro'
GRUB_CMDLINE_LINUX_DEFAULT="quiet acpi_osi=! acpi_osi='Windows 2009' "
GRUB_CMDLINE_LINUX="reboot=bios"
```
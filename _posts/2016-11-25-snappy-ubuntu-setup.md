---
title:  零基础为树莓派安装操作系统 Snappy Ubuntu Core
---

因为要做个物联网方面的事情，所以给入手的树莓派装个操作系统，看到比较时新的Snappy Ubuntu Core也想尝试一把，可惜网上资料太少，所以此处把我的安装过程，给刚入门的朋友介绍一下。

>
先提前告诉大家，这个系统我装完就卸载了，并不是说它不好，而是因为它只是个核心，没有图形界面，对于我目前来说，面对一个只有指令交互的界面还是很崩溃的。

<!--more-->

## Snappy Ubuntu Core

Snappy Ubuntu Core 有不少特点，可以到[这里](http://www.techweb.com.cn/network/system/2015-11-20/2229163.shtml)详细了解，此处不赘述。

在树莓派[官网](https://www.raspberrypi.org/downloads/)，我们可以看到很多可选镜像，此处选择Snappy Ubuntu Core即可。

进入官网[这个页面](https://developer.ubuntu.com/en/snappy/start/raspberry-pi-2/)，已经有了系统安装的详细步骤，但是初次使用者可能会有不理解的地方，本文以中文形式，利用Mac对树莓派进行系统安装，并详细介绍过程。

## 前期准备

- 创建ubuntu账户。安装成功后，启动系统会进行账户验证，所以要有这个步骤。这也是Snappy Ubuntu Core挺烦人的一点。

- [导入SSH Key到你的Ubuntu账户内](https://login.ubuntu.com/ssh-keys)。如果用过GitHub大家应该对这个操作很好理解。比如我把当前自己Mac上[生成的SSH Key](https://help.ubuntu.com/community/SSH/OpenSSH/Keys)导入到账号内，那么对Snappy Ubuntu Core进行远程SSH登陆时，就会方便很多。你可能说，并不想远程登录，只想直接操作树莓派，然而系统最初默认的是ssh登陆，是不是又一个不太爽的地方。

- 准备输入输出设备。也就是键盘和显示器，但是注意一点，利用HDMI输出的时候，请把config.txt文件中，hdmi_safe=1前面的注释符号#去掉。

## 下载安装

### 下载

首先先根据树莓派类型，选择下载文件，并解压出镜像。 
 
- [树莓派2](http://releases.ubuntu.com/ubuntu-core/16/ubuntu-core-16-pi2.img.xz)  
- [树莓派3](http://releases.ubuntu.com/ubuntu-core/16/ubuntu-core-16-pi3.img.xz)

### 安装 

这一步我们会把镜像文件烧录到sd卡上。

####  1. 查看sd信息。
内存卡插入到电脑，查看电脑disk信息，查看指令如下： 

```
    diskutil list
```
你可能会得到如下磁盘信息：

```
/dev/disk0
  #:                       TYPE NAME                    SIZE       IDENTIFIER
  0:      GUID_partition_scheme                        *500.3 GB   disk0
/dev/disk2
  #:                       TYPE NAME                    SIZE       IDENTIFIER
  0:                  Apple_HFS Macintosh HD           *428.8 GB   disk1
                                Logical Volume on disk0s2
                                E2E7C215-99E4-486D-B3CC-DAB8DF9E9C67
                                Unlocked Encrypted
/dev/disk3
  #:                       TYPE NAME                    SIZE       IDENTIFIER
  0:     FDisk_partition_scheme                        *7.9 GB     disk3
  1:                 DOS_FAT_32 NO NAME                 7.9 GB     disk3s1
```
注意保证sd卡到格式是FAT_32，上述例子中sd卡对应的是 /dev/disk3 。

####  2. 卸载sd卡。
指令如下：

```
    diskutil unmountDisk /dev/disk<disk number you wrote down above>
```

在本文的例子中，我们的指令是：

```
    diskutil unmountDisk /dev/disk3
```

卸载成功时，会有

```
    Unmount of all volumes on disk# was successful
```

#### 3. 烧录。
将镜像烧录进sd卡，指令如下：

```
    sudo dd if=~/Downloads/ubuntu-core-16-pi3.img of=/dev/rdisk<disk number you noted above> bs=32m
```
本文例子中：

```
    sudo dd if=~/Downloads/ubuntu-core-16-pi3.img of=/dev/rdisk3 bs=32m
```

注意：最后32后面的小写字母m，代表MB，官网大写但是实践中并不能通过，注意改为小写。

#### 4. 成功
 
 过一段时间后，你会得到类似如下信息，代表烧录成功。
 
```
    3719+1 records in
    3719+1 records out
    3899999744 bytes transferred in 642.512167 secs (6069924 bytes/sec)
```
   
### 第一次启动

#### 1. 配置
第一次启动需要进行相关配置，首先会出现 “Press enter to configure”，然后会有“Start”进入配置。按照指示这里会要求你配置网络和ubuntu账户信息。所以前面所要求的输入输出设备和ubuntu账号都必不可少。

注意：请操作时给树莓派连接有线网，配置过程中需要网络连接，和一般操作系统安装不太一样。  

安装到最后，会出现下面的提示，告诉你可以用你的ubuntu账号访问该树莓派。是不是觉得奇怪，为什么要ssh访问，直接开机键盘交互不就好了，然而并没有这么简单，这个操作系统默认是你要从另一个终端上网，利用ssh登陆这个操作系统的（可能是该系统主要是为物联网器件开发，所以有这个特点）。
```
    This device is registered to <Ubuntu SSO email address>.
    Remote access was enabled via authentication with the SSO user <Ubuntu SSO user name>
    Public SSH keys were added to the device for remote access.
```

#### 2. 登陆

上面已经登陆方式和原因，登陆指令如下：

```
    ssh <Ubuntu SSO user name>@<device IP address>
```
至于user name请自己查看个人账户信息。

其实也可以本地登陆的，不过要进行一个操作，指令也很简单：

```
    sudo passwd <account name> 
```
然后设置一个密码，之后你就可以本地登陆了。


---
[参考官网](https://developer.ubuntu.com/en/snappy/start/raspberry-pi-2/)
欢迎转载，请注明出处：hewei.site/snappy-ubuntu-setup

    
    














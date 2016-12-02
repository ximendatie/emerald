---
title:   零基础用Mac为树莓派3安装Ubuntu mate
---




用Mac为树莓派安装Ubuntu mate十分类似于安装Snappy Ubuntu Core，不过Ubuntu的图形界面对大多数人来说还是很友好的，本文讲述Mac下怎么为树莓派安装Ubuntu mate

<!--more-->

1. 首先去官网下载用于树莓派的unbuntu镜像。https://ubuntu-mate.org/raspberry-pi/

2. 使用Mac自带的解压缩工具解压后缀为xz的文件，得到img文件

3. 使用[SDFromatter](https://www.sdcard.org/downloads/formatter_4/eula_mac/index.html)工具将SD卡格式化

4. 使用df -h命令查看挂载的u盘卷，本例为disk2s1

5. 卸载该卷： `sudo diskutil umount /dev/disk2s1` (卸载指令后不要拔掉SD卡)

6. 使用diskutil list来确认设备

7. 使用dd命令将系统镜像写入：

```
    sudo dd bs=4m if=ubuntu-mate-16.04-desktop-armhf-raspberry-pi.img of=/dev/rdisk2
```

>
注意: 最后的是rdisk2，不是disk2，更不是disk2s1。
（说明：/dev/disk2s1是分区，/dev/disk2是块设备，/dev/rdisk2是原始字符设备）

经过漫长的等待，刷入完毕，卸载设备。将sd卡插入树莓派，进行常规安装。
卸载指令：

        sudo diskutil unmountDisk /dev/disk2
        
        
        
---
[转载原文](https://1024coder.com/topic/40)




---
title:   npm install 本地安装与全局安装的区别
---


npm的包安装分为本地安装(local)、全局安装(global)两种，命令行上的区别只是有没有-g而已。

<!--more-->

比如:

    npm install grunt # 本地安装
    npm install -g grunt-cli # 全局安装
    
这两种安装方式有什么区别呢?从npm官方文档的说明来看，主要区别在于:

### 本地安装

1. 将安装包放在 ./node_modules 下(运行npm时所在的目录)
2. 可以通过 require() 来引入本地安装的包

### 全局安装

1. 将安装包放在 /usr/local 下
2. 可以直接在命令行里使用


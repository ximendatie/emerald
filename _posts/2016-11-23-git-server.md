---
title:  GitStack 从零开始windows搭建git服务器
---



目前流行的版本控制系统实在太方便了，尤其是分布式的git，以前比较常用的SVN是集中式的现在已经逐渐被git所取代。对于版本控制系统的仓储，一般来说GitHub已经基本满足了所有需求。如果你对自己代码安全性要求比较高，而又不想购买GitHub的私有仓储，那么可以尝试自己搭建一个git服务器。

<!--more-->

### 前言

 - 这篇博客虽然是windows搭建教程，大家要是有条件还是推荐在[Linux下搭建服务器](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137583770360579bc4b458f044ce7afed3df579123eca000/)，windows对软件依赖比较高，几乎没有指令发挥空间，功能实现也会受限于软件本身的功能。
 
 - 搜索相关教程时，多数采用copSSH结合msysGit的形式搭建git服务器，但过程繁琐容易出错。试验后，本文采取十分简洁的GitStack进行安装。


### 安装GitStack

1. 官网进行[下载](http://gitstack.com/download/)
2. 下载后如同其他windows软件一样，只需双击安装即可

注意：

- 目前GitStack只支持
  - Windows Server 2008
  - Windows Server 2008 R2
  - Windows Vista
  - Windows 7
- 安装路径中不要包括空格，所以不建议安装到C:\Program Files下，默认是安装到C:\GitStack下
- GitStack只需要服务端安装，客户端无需安装

### 配置GitStack

在服务器上，可以通过开始菜单找到GitStack打开，也可以直接打开浏览器，在地址栏里输入http://localhost/gitstack/打开登录界面。

另外，也可以通过server机的IP地址来登录，如server的IP地址为：192.168.0.105，则可以直接在浏览器的地址栏中输入 http://192.168.0.105/gitstack/ 打开登录界面(注意在客户机上只能使用这种方式来打开登录界面，通过ipconfig可以查看本机的IP地址)。

初始状态下，默认的登录账户为admin，登录密码也为admin。管理员登录后可在Settings->General中修改admin的登录密码。

勾选Enable web based repository browsing选项开启在浏览器中直接查看Git仓库的内容。

另外还有两个Repositories和Users & Groups两个界面，其中在Repositories中可以在服务器上创建项目的裸仓库，直接输入仓库名（如输入ProjectRepos），然后点击Create按钮即可（会在服务器C:\GitStack\repositories下创建一个ProjectRepos.git裸仓库），创建好的仓库也会在Repositories中显示出来，并显示出该仓库的clone的地址git clone http://localhost/ProjectRepos.git，之后就可以在Action下通过浏览器查看仓库、添加用户/Group并设置用户/Group权限等。

在Users & Groups中，Users下是用来创建用户或修改用户密码等，每个用户对应一个Username和其Password，已有的用户会在上面的列表中显示出来；Groups下用于创建组，可以在每个Group下添加或移除用户，已有的Group也会在列表中显示出来。

### 牛刀小试

上述已经在服务器上创建了一个ProjectRepos.git裸仓库，现在我们在服务器上来克隆该仓库。

    cd d:
    mkdir project
    cd project
    git clone http://192.168.0.105/ProjectRepos.git
    
默认的是80端口，可以修改为其他端口。 这里，会提示输入用户名和密码，注意输入的用户名和密码不会被显示出来。

cd LocalRepos 进入了工作目录，我们可以添加文件到工作区，并提交到本地仓库中。

然后，将本地修改推送到服务器的仓库里：git push origin master，这里会提示输入用户名和密码，注意输入的用户名和密码不会被显示出来。 通过git remote -v，我们可以查看origin对应的服务器上的仓库地址。

这时打开GitStack，可以看到服务器上仓库有了提交的内容。

### 在客户机上克隆或提交代码

先在客户机上安装git客户端

    cd d:
    mkdir project
    cd project
    git clone http://192.168.0.105/ProjectRepos.git
    
这里，会提示输入用户名和密码，注意输入的用户名和密码不会被显示出来。

cd LocalRepos 进入了工作目录，我们可以添加文件到工作区，并提交到本地仓库中。

然后，将本地修改推送到服务器的仓库里：git push origin master，这里会提示输入用户名和密码，注意输入的用户名和密码不会被显示出来。 通过git remote -v，我们可以查看origin对应的服务器上的仓库地址。

这时打开GitStack，可以看到服务器上仓库有了提交的内容。

在客户机上也可以打开GitStack，直接在浏览器的地址栏中输入http://192.168.0.105/gitstack/打开登录界面，当然这需要知道管理员密码。

（这里要注意的是，要保证在客户机上能够成功打开GitStack或者从服务器上克隆仓库，必须将服务器的防火墙关闭，否则在客户机上的这些操作就会失败。这个问题一直困扰了我好几个小时。）

可见，服务器和客户机在操作上已经没有什么区别了，这正是Git作为分布式版本控制系统的体现。


---

[文章参考](http://shanewfx.github.io/blog/2012/05/03/git-server-based-on-gitstack/)
 









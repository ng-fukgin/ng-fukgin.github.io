---
layout: post
title: frp+ssh实现远程连接配置教程
categories: 教程
description: 解决 IntelliJ IDEA 启动报 Unsupported Java Version 的问题
keywords: FRP, ssh,远程连接
---

>声明：本文档并非我本人自愿编写，如有漏洞，最好不要找我（当然如果你搬出了詹詹，那可以找我）。


## 1.   配置ssh
### 1.1.    SSH 分为 Client 和 Server
Ubuntu 默认自带SSH Client，端口号为22。如果没有，可通过sudo apt-get install openssh-client 来安装。如果作为Server端则需要安装SSH Server。
### 1.2.   安装SSH Server并启动
+ 查看SSH Server是否安装   
 `dpkg -l|grep ssh`   
 如下图，SSH Client和SSH Sever都安装了   
 ![](/images/posts/frp+ssh/SSH_installed.png) 
+ 安装ssh sever    
`sudo apt-get install ssh `    
如果失败,那就使用    
`sudo apt-get install openssh-server`
+ 查看是否安装成功&&查看是否启动成功    
`dpkg -l|grep ssh`    
图中结果说明SSH Server安装成功    
![](/images/posts/frp+ssh/SSH_suess.png)     
+ 启动 SSH Server    
`ps -e|grep ssh`    
有sshd说明SSH Server已经启动成功    
![](/images/posts/frp+ssh/SSH_qd.png)     
如果没有启动，可通过以下两种方式启动：    
```
sudo /etc/init.d/ssh start
sudo service ssh start
```
### 1.3.  SSH Server相关配置
SSH Server配置文件在 `/etc/ssh/sshd_config`          
`sudo vim /etc/ssh/sshd_config`    
+ 这里可以设置SSH Server 端口，默认是22      
![](/images/posts/frp+ssh/sshd_config1.png)      
+ 允许root用户以任何认证方式登录      
屏蔽`PermitRootLogin without-password`，增加`PermitRootLogin yes`    
![](/images/posts/frp+ssh/sshd_config2.png)  
+ 重启SSH Server   
```
sudo /etc/init.d/ssh stop
sudo /etc/init.d/ssh start
```
或者   
`sudo service ssh restart`     
![](/images/posts/frp+ssh/ssh_restart.png)    
+ 尝试是否能连通    
使用方法      
`ssh username@ip `   
![](/images/posts/frp+ssh/ssh_if_ping.png)     

## 2.  配置frp

### 2.1.  在客户端配置frp
  + 点击[打开](https://github.com/fatedier/frp/releases)，可以看到有很多包，下载适合自己系统的包，然后解压。
  +  然后cd 到frp文件夹，查看文件    
    ![](/images/posts/frp+ssh/frp_flo.png)  
  + 再之后编辑frpc.ini文件     
      ![](/images/posts/frp+ssh/frp_ed_frpc.png)  

  +上图是我配置的文件，然后在网上找了个说明，可以看看    
  ![](/images/posts/frp+ssh/frp_e_frpc.png)    

### 2.2.  在客户端后台开启运行 frp 服务

`$ nohup ./frpc -c frpc.ini >/dev/null 2>&1 &`   
说明：`>/dev/null 2>&1 &`，表示丢弃。    
如果在服务运行时修改frpc.ini配置后，需重启frpc服务才能生效。执行以下命令查询frpc 运行进程，并使用kill -9命令停止服务。

    ```
    ps -aux|grep frp| grep -v grep  
     # 查询 frpc 的运行进程ID,并 kill 停止服务.
     kill -9 1108
     ```
     
如因误操作等原因出现多个进程，可能会导致 frpc 无法使用正确的 frpc.ini 配置，需全部停止，再次启动即可。    
如果 frpc 的服务器识别明在 frps 服务器上有冗余重复也会导致配置失败，需要进入公网服务器的 dashboard_port 端口进行查看，dashboard_port 端口和 dashboard 界面的管理员账号密码设置如下。    
另外，记得需要赋予 frp 目录可执行权限，否则会出现 exit 126 报错。

### 2.3.  在服务端配置frp
   + 首先你 **花money** 在[阿里](https://cn.aliyun.com/)，[腾讯](https://cloud.tencent.com/)，华为等购买云服务器，然后配置好，在云服务器上下载frp文件（可以在自己的电脑上下载，然后通过ssh传上去）
  + 然后cd到frp文件夹，运行`nohup ./frps -c frps.ini >/dev/null 2>&1 & `
  + 这个`frps.ini`文件默认是配置好的，基本上不同改











### 2.4.  测试frp+ssh

    'ssh -p [端口] 用户名@公网ip'


​    
![](/images/posts/frp+ssh/ssh_ip.png)  

连接上了，完美！！！

## 3.  用户体验
&emsp;&emsp;2021年3月22日，天气晴朗，万里无云。    
&emsp;&emsp;我和刘他们高高兴兴蹦蹦跳跳地来到公司，当我们跟领导以及同事初步认识后，我回到了我的座位坐下。可是我的屁股还没有坐热，我们部门的leader就发了一封邮件给我，然后绕了一个弯过来教我使用ssh。    
&emsp;&emsp;当他走过来，打开邮件把ssh xxx@xxxx 复制粘贴到cmd上时，我就知道这个leader非常地不简单，因为他的命令没有一个是对的。搞了一会儿，他没有搞定，走回了座位，然后我自己研究下，才发现他给我的命令都是在中文状态下的。过了不到30秒，他回来了，然后看了一下我的屏幕，说：”啊，你知道怎么回事啦”。然后转身就走了，去教刘他们用了。    
&emsp;&emsp;等他走了，我就自己研究下这个ssh，其实一开始我是很抵触这个ssh，因为没有图形界面，纯命令行操作，用惯了图形界面的我很不习惯。可是当我用了大概一个多钟，我突然发现这个ssh可真是宝贝啊。    
&emsp;&emsp;ssh可以实现多人连接，前提是不同的人使用不同的用户去连接，不然好像会冲突，所以你们在用的时候可以在ubuntu上创建多一点用户。不同的人连接ssh互不干扰，可以同时炼丹。还有，当网络很慢的时候，我们用teamviewer，todesk之类的连接，连接老半天都连不上，用ssh秒连，为什么？因为ssh不耗那么多带宽啊。而且只要电脑不死机，ssh基本上一辈子都能连上。这不是宝贝是什么？    
&emsp;&emsp;然后说说这个frp，frp的作用就相当于一个跳板，他连接了内网和外网，用户可以通过这个frp连接内网的一切资源，不单单是用来做外网ssh用，可以用来映射ftp，可以在学校外连接学校的内网。举个例子：在很久很久，有一段18级之后的同学不知道的时光，学校的正方教务系统外网连不上，这时候就可以通过frp连接学校的一台“肉鸡”来登录正方教务系统了。




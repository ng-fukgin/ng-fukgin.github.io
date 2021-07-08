---
layout: post
title: 利用github创建自己的博客-简易
categories: 教程
description: 利用github创建个人博客
keywords: github,blog,博客
---

>一个人至少拥有一个梦想，有一个理由去坚强。心若没有栖息的地方，到哪里都是在流浪。
—三毛


## 1. 注册github账号
注册github账号就不啰嗦了，自己去搞就好了

## 2. 开始搭建博客
### 2.1. 新建一个Repositories
Repositories相当于一个仓库，存放我们的代码
+ 第一步，点击github右上角的+，然后点击new repository    
![](/images/posts/github_blog/new_repositiory.PNG)

+ 然后给自己的Repositories取一个名字，注意：名称格式最好为：[用户名.github.io]

![](/images/posts/github_blog/new_repositiory1.PNG)

### 2.2. 下载GitHub桌面版
GitHub桌面版为了避免使用命令行，你也可以不下，使用命令行上传/下载代码

+ 下载安装好后，登录自己的账号,然后选择File下的Clone repositories.
![](/images/posts/github_blog/v2-c5f165c7056bf8a1e81c98880e6bc3b7_720w.jpg)

+ 然后将你刚刚建好的repositories的url复制过来，点击Clone。一般存放的默认文件夹为

C:\Users\计算机名\Documents\GitHub。
![](/images/posts/github_blog/v2-faf4513657fd78aad02687c02ba2e1c0_720w.jpg)

+ clone完之后你会发现C:\Users\计算机名\Documents\GitHub路径下多了一个文件夹，如图：

![](/images/posts/github_blog/v2-c73a4bec66a84957dd8b069b43f177d1_r.png)

+ 然后将该文件夹里的文件都删掉，如果有.git文件，需要保留.git文件

### 2.3. 下载博客模板
+ 网上好看的模板一大堆，大家搜一下就有了，我的模板是从github的[mzlogin](https://github.com/mzlogin/mzlogin.github.io)下载下来的。

+ 然后把下载下来的模板文件复制到本地那个库文件夹上

+ [mzlogin](https://github.com/mzlogin/mzlogin.github.io)的模板很好懂,博客主要存放在_posts 文件夹上，大家修改_config.yml这个配置文件就可以把博客变成自己的了

+ 修改好博客文件之后就可以上传到github上了

### 2.4. 上传文件到giuhub 并配置网页

+ 打开GitHub桌面版。你会发现有非常多的changes，如图   
![](/images/posts/github_blog/v2-3b08ca2dfb6215d86595c4fdc9fdb7f7_720w.jpg)
+ 在summary中随意填一些内容，然后再点击commit to master。   
 注意：可能由于墙或者什么网络问题，上传有可能会失败或较久，需要多试几次。     
![](/images/posts/github_blog/v2-dcf13d09dfd1b34fb474ecbdcd404567_720w.jpg)

+ 然后回到自己的github上，点开自己的博客库，点击设置 
![](/images/posts/github_blog/blog_setting1.PNG)

+ 找到 GitHub Pages ，然后Source那里选择master 点击save 

![](/images/posts/github_blog/blog_setting2.PNG)

+ 大功告成！打开[https://用户名.github.io/](https://txwfj.github.io/)应该就可以看到自己的博客了，如果不行，就等一下


## 3.**感谢**
+ **本文大部分内容是参考 [我是前端切图仔](https://zhuanlan.zhihu.com/p/28321740)的知乎内容**

+ **本博客基于[mzlogin](https://github.com/mzlogin/mzlogin.github.io)改动而成,饮水思源**
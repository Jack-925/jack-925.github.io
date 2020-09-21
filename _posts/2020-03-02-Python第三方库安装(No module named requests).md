---
layout: post
comments: true
categories: python
tags: [python,第三方库]
---
# ModuleNotFoundError: No module named 'xxx'
相信很多同学在使用python用pip install xxx时各种被墙。。在这里给大家推荐个网站
https://www.lfd.uci.edu/~gohlke/pythonlibs/
里面有非常丰富的python库，诸如：**pymysql**(注意若导入此包，程序名不可也叫做pymysql，曾经踩过的bug。。）以及爬虫非常好用的**requests**包。
网站界面如图所示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200302002116105.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NwcHBwNjY=,size_16,color_FFFFFF,t_70)
## ctrl+f 即可开启搜索窗口
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200302002352898.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NwcHBwNjY=,size_16,color_FFFFFF,t_70)
##  1、点击下载后将文件名后缀改为zip
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200302002630218.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NwcHBwNjY=,size_16,color_FFFFFF,t_70)
## 2、解压文件后，将无dist-info字样的文件夹复制到Python安装目录下的Lib文件夹中（一般Python默认安装在C:\Program Files\中）

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200302002815144.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NwcHBwNjY=,size_16,color_FFFFFF,t_70)
## 我的是python3如下图所示，所有第三方库都需放在Lib目录下
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020030200303987.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NwcHBwNjY=,size_16,color_FFFFFF,t_70)放到Lib目录下便可以使用啦

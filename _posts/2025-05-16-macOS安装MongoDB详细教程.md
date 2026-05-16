---
layout: post
title: "macOS安装MongoDB详细教程"
date: 2025-05-16
tags: [MongoDB]
---

随着macOS版本的迭代升级，修改相关的系统文件变得越来越困难。即使在关闭SIP保护之后，系统的根目录仍然无法修改删除，这可能会引起一些软件的权限不够从而导致配置失败。以前的MongoDB安装是使用homebrew命令一键配置，但是我使用homebrew安装的时候出现了严重的权限问题。MongoDB的配置文件无法再写入到系统的一些敏感位置，所以相关服务老是启动失败，于是只能寻找新的方法，最终配置成功了，下面是详细的配置教程。结合了网上的一些文字教程和视频教程，参考链接如下：

1.mongoDB官网：https://www.mongodb.com

2.【软件安装｜macOS下mongoDB的安装全过程】 https://www.bilibili.com/video/BV1wr4y1e7rw

3.【软件安装｜macOS下mongoDB的安装与报错处理】 https://www.bilibili.com/video/BV14b7rzxEdh

（一）下载并安装MongoDB
先去官网下载MongoDB的压缩包，并且解压出来：
https://www.mongodb.com/zh-cn/products/self-managed/community-edition
![image](https://img2024.cnblogs.com/blog/3599182/202508/3599182-20250813165838283-584764957.png)
下滑，基于自己的Mac处理器架构找到选合适的版本，再点击download
![image](https://img2024.cnblogs.com/blog/3599182/202508/3599182-20250813165925966-644267318.png)
下载好后点击解压缩，并且重新命名压缩包为mongodb，并拷贝
![image](https://img2024.cnblogs.com/blog/3599182/202508/3599182-20250813165946247-838163725.png)
![image](https://img2024.cnblogs.com/blog/3599182/202508/3599182-20250813165953696-509573454.png)
![image](https://img2024.cnblogs.com/blog/3599182/202508/3599182-20250813170003136-204123258.png)
前往根目录，按住电脑command+shift+.键位，展示隐藏文件，前往/usr/loacl文件夹下，粘贴MongoDB文件到这里
![image](https://img2024.cnblogs.com/blog/3599182/202508/3599182-20250813170025547-2109312736.png)

（二）配置环境变量
现在的macOS基本上使用的是Zsh，它是bash的一个变体，扩展了功能，但是总归不是bash。bash现在广泛用于Linux/GNU系统当中，所以配置文件应该是用户目录下的.zshrc文件。

打开终端，输入：
`vim .zshrc`

在文件末尾添加以下环境变量，之后保存并退出：
`export PATH="/usr/local/mongodb/bin:$PATH"`

然后在终端输入一下命令使得环境配置生效：
`source .zshrc`

输入命令看安装是否成功，如果显示类似下图一样就是成功了：
`mongod --version`
![image](https://img2024.cnblogs.com/blog/3599182/202508/3599182-20250813170236611-2088345257.png)

（三）建立数据和日志文件
注意，这里问题来了。由于macOS的系统安全性（封闭性）不断提升，已经没有办法修改一些系统的底层文件，就连关闭SIP保护、挂载系统文件之后都不行（至少我做不到，有大佬能做的话欢迎批评指正），这会导致MongoDB无法创建自己的数据和日志文件，也就无法真正启动服务。所以我们只能自己手动创建相关文件。

继续使用终端命令，完成文件创建
`cd /usr/local/mongodb`
`mkdir data log`
`sudo chown {$USER} /usr/local/mongodb/data`
 输入密码
`sudo chown {$USER} /usr/local/mongodb/log`
这里的{$USER}是你电脑设置的用户名，你需要替换成你自己的名字。如果不是很清楚可以运行:
`whoami`
输出的结果就是你的用户名

（四）启动Mongo服务
首先进入MongoDB的安装路径：
`cd /usr/local/mongodb`
再输入命令启动服务，这里需要root权限才能启动，否则权限不够就会报错ERROR：
`sudo mongod --fork --dbpath data --logpath log/mongo.log --logappend`
如果配置没错的话就会看到创建了子进程，服务正常启动，如下图所示：
![image](https://img2024.cnblogs.com/blog/3599182/202508/3599182-20250813170618928-1387607209.png)

（五）进入Mongo环境
原来的版本是在终端输入mongo命令就可以进入环境，但是随着版本升级这个命令已经被弃用，现在输入mongo之后会显示找不到命令，应该转而使用mongosh命令，也就是MongoDB Shell，需要在官网下载https://www.mongodb.com/try/download/shell：
![image](https://img2024.cnblogs.com/blog/3599182/202508/3599182-20250813170637870-1884261902.png)
下载之后安装，同上面一样的方法，重命名为mongosh，并拷贝一份到/usr/local下面
![image](https://img2024.cnblogs.com/blog/3599182/202508/3599182-20250813170800396-394174834.png)
用vim命令再次打开.zshrc：
`cd ~`
`vim .zshrc`
在里面添加一条mongosh环境变量:
`export PATH=/usr/local/mongosh/bin:$PATH`
保存配置然后应用更改：
`source .zshrc`
现在MongoDB Shell就安装配置成功了，现在所有的mongo命令全部替换为mongosh命令，此时在终端键入：
`mongosh`
![image](https://img2024.cnblogs.com/blog/3599182/202508/3599182-20250813171000530-1704917365.png)
此时就可以进入mongo环境，创建数据库、删除数据库等等操作都可以进行，跟MySQL差不多，输入exit就可以退出

有时候，macOS 可能会阻止mongosh在安装后运行。 如果在启动mongosh时出现安全错误，表明无法识别或验证开发者的身份，请执行以下操作：

打开“系统偏好设置” 。

选择“安全和隐私”窗格。

在常规标签页下，单击有关mongosh的消息右侧的按钮，点击按钮标记为“仍然运行”。

（六）管理MongoDB数据库
其实MongoDB官方提供了一些能够图形化管理数据库的工具，但是很多人都会习惯使用Navicat来集中管理所有的数据库，我也更喜欢使用Navicat，下面是使用Navicat的教程，跟MySQL差不多

首先就是打开Navicat，点击左上角的连接
![image](https://img2024.cnblogs.com/blog/3599182/202508/3599182-20250813171036081-718940532.png)
选择MongoDB，并点击下一步
![image](https://img2024.cnblogs.com/blog/3599182/202508/3599182-20250813171050297-204423242.png)
连接名称可以是mongodb，单击测试连接检查一下
![image](https://img2024.cnblogs.com/blog/3599182/202508/3599182-20250813171106021-2084301953.png)
双击左侧区域就可以进行管理操作了，可以进行SQL查询
![image](https://img2024.cnblogs.com/blog/3599182/202508/3599182-20250813171123252-1197063982.png)
---
layout: post
title:  "python3 爬虫"
date:   2019-07-21 21:58:25 +0800
---

# python3爬虫学习笔记

---

学习书籍 《Python3网络爬虫开发实战》崔庆才。微店 25 元图书包邮。

书籍配套视频 34 课时，共 20 小时。地址： B 站。

配套让听课有不清楚的地方立即可以查。书写得清楚明了，是名校学生的清晰思路。非常推荐！

---

<!-- TOC -->

- [python3爬虫学习笔记](#python3爬虫学习笔记)
- [为什么学习爬虫](#为什么学习爬虫)
- [安装](#安装)
    - [编程环境安装](#编程环境安装)
    - [数据库安装](#数据库安装)
    - [多版本共存的环境配置](#多版本共存的环境配置)
    - [爬虫常用库的安装](#爬虫常用库的安装)
- [爬虫基础](#爬虫基础)
    - [基本原理](#基本原理)
    - [使用requests](#使用requests)
- [实战项目-抓取豆瓣某本书的评价](#实战项目-抓取豆瓣某本书的评价)
- [框架](#框架)
- [分布式](#分布式)

<!-- /TOC -->



# 为什么学习爬虫

- 学习动力

随着年龄逐渐增长，物质与精神需求日益增长，迫切地需要更优质的资源。而最大的爬虫系统“百度”魏则西的大学生死亡事件，华为2019年被美国禁止出口，告诉我们：**技术必须掌握在自己手里！**

技术掌握在自己手里决定着我们必须对机制更好的了解并促进我们正确地判断信息来源的正确与否，利用技术快速高效获取资源，这种资源可能包括着付费内容。例如： VIP 音乐，自动批量文档下载。


**网络爬虫就是一种高效的信息采集利器，快速、准确地采集我们想要的各种数据资源。**

这已经成为当代人的基本技能。就像会算数，开车，打字，已经不是谋生手段，而是必备技能。

- 学习本质

浏览器是我们日常所用的，其逻辑已经非常了解，爬虫的编程则是对**思维和场景的强化**，是**最快上手的小编程项目**，而且结果还能促进日常工作与生活效率。对编程学习极有帮助！


# 安装

学习一门技术，可能最开始就卡再环境配置安装上，如果过程花费太多时间，则会消耗大量的学习劲头进而影响学习效率。



## 编程环境安装

- **ubuntu18** （linux系统）下，使用**命令行**安装 **python3**

注意：我使用的是虚拟机的 ubuntu 系统。因为 windows的系统内容太多，而虚拟机我有好几个专事专用，配置的资源环境近十年内社区是专人维护的，**镜像体积小，复用性大**。

命令行下**安装软件方便**：在 windows 里安装程序，yes ,下一步，next ,……，而命令行有个命令 -y 即可自动对所有过程中出现的问题默认选项进行 yes 操作，而且一行命令同时可以安装几个程序，极大解放安装注意力。

- 数据库在 windows 下安装是需要配置端口号的。


(1)基础依赖库安装

没有这些库，后面步骤的安装可能会报错！

>apt install -y python3-dev build-essential libssl-dev libffl-dev libxml2 libxml2-dev libxslt1-dev zlib1g-dev libcurl4-openssl-dev


解释：代码里的 -y 选项是为了安装过程中询问而做的自动批量回答 yes 而用的。会询问什么呢？例如需要额外的占用多少 MB 空间。

(2)安装 python3 编译环境


>apt install -y python3


(3)安装 pip

pip 是通用的 Python 包管理工具。  
提供了对 Python 包的查找、下载、安装、卸载的功能。


>apt install -y python3-pip

检查是否安装成功，输入 pip3
提示 pip 有关的信息就成功了，例如下面的提示内容。
```
Usage:   
  pip <command> [options]

Commands:
  install                     Install packages.
  download                    Download packages.
...
```


## 数据库安装

爬虫的队列需要保存。

- MongoDB3.6  

非关系型数据库

>apt install -y  mongodb

- Redis 
   
非关系型数据库
>apt -y install redis-server

检测是否安装成功，输入 

```
redis-cli
```
进入命令行模式，输入键值 
```
set 'a' 'b'
```
回车后，会提示 OK
```
127.0.0.1:6379> get 'a'
```
提示 "b"则说明安装成功。

若想 Redis 具有远程连接功能，还需要修改配置文件
```
vi /etc/redis/redis.conf
```
注释 bind 127.0.0.1 ::1 这一行，后即可具有远程连接功能。

```
#requirepass foobared
```
使用 vim 编辑器找 /foobared 到这一行后，回车，输入 i 进入编辑模式。  
取消这行的注释符，并将 foobared 这个词 换成你想设置的密码，我将其设置为 123456  
重启 Redis 服务，使配置内容生效。  
```
/etc/init.d/redis-server restart
则提示  [ ok ] Restarting redis-server (via systemctl): redis-server.service.
```
```
Redis 服务的停止命令为 sudo /etc/init.d/redis-server stop
启动命令为 sudo /etc/init.d/redis-server start
```
启动 redis 命令行，输入 get内容再次获得a，提示没有权限
```
redis-cli
127.0.0.1:6379> get 'a'
(error) NOAUTH Authentication required.
                 认证         需要
```
说明在启动命令行时，需要加密码了。
```
redis-cli -a 123456
127.0.0.1:6379> get 'a'
"b"
```

- MySQL

简单已用，轻量型，关系型数据库

>apt install -y mysql-server mysql-client

会提示 输入用户名和密码

```
mysql -uroot -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.26-0ubuntu0.18.04.1 (Ubuntu)

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)
mysql> use mysql;
Database changed
mysql> select * from db;
+-----------+--------------------+---------------+-------------+-------------+-------------+-------------+-------------+-----------+------------+-----------------+------------+------------+-----------------------+------------------+------------------+----------------+---------------------+--------------------+--------------+------------+--------------+
| Host      | Db                 | User          | Select_priv | Insert_priv | Update_priv | Delete_priv | Create_priv | Drop_priv | Grant_priv | References_priv | Index_priv | Alter_priv | Create_tmp_table_priv | Lock_tables_priv | Create_view_priv | Show_view_priv | Create_routine_priv | Alter_routine_priv | Execute_priv | Event_priv | Trigger_priv |
+-----------+--------------------+---------------+-------------+-------------+-------------+-------------+-------------+-----------+------------+-----------------+------------+------------+-----------------------+------------------+------------------+----------------+---------------------+--------------------+--------------+------------+--------------+
| localhost | performance_schema | mysql.session | Y           | N           | N           | N           | N           | N         | N          | N               | N          | N          | N                     | N                | N                | N              | N                   | N                  | N            | N          | N            |
| localhost | sys                | mysql.sys     | N           | N           | N           | N           | N           | N         | N          | N               | N          | N          | N                     | N                | N                | N              | N                   | N                  | N            | N          | Y            |
+-----------+--------------------+---------------+-------------+-------------+-------------+-------------+-------------+-----------+------------+-----------------+------------+------------+-----------------------+------------------+------------------+----------------+---------------------+--------------------+--------------+------------+--------------+
2 rows in set (0.00 sec)

为了可以远程访问，需要修改配置文件
vi /etc/mysql/mysql.conf.d/mysqld.cnf
注释此行 bind-address = 127.0.0.1
重启生效
service mysql restart
```


## 多版本共存的环境配置

|版本冲突问题|实质|
|:---:|:---:|
|python2 与 python3 |名字的冲突|

- 电脑中可能存在多个版本 python 编译库，编程者也需要多个版本库编译程序查看运行结果是否与目标一致。

- 执行编译的程序是可执行文件 exe 。 exe 文件只能在当前目录路径下执行。

- 若想在目录外也执行，就需要设置“环境变量”。

|编号|解决方法|
|---|---|
|1|取独特的名字。**制定 number**。复制文件取新名|
|2|默认的名字，**指定环境变量顺序**，优先调用默认|

```
输出环境变量

teenie@ubuntu:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin

查找命令版本及路径

teenie@ubuntu:~$ whereis python
python: /usr/bin/python /usr/bin/python2.7 /usr/bin/python3.6-config /usr/bin/python2.7-config /usr/bin/python3.6 /usr/bin/python3.6m-config /usr/bin/python3.6m /usr/lib/python2.7 /usr/lib/python3.6 /usr/lib/python3.7 /etc/python /etc/python2.7 /etc/python3.6 /usr/local/lib/python2.7 /usr/local/lib/python3.6 /usr/include/python2.7 /usr/include/python3.6 /usr/include/python3.6m /usr/share/python /usr/share/man/man1/python.1.gz


查找命令详细路径

teenie@ubuntu:~$ whereis python3
python3: /usr/bin/python3.6-config /usr/bin/python3 /usr/bin/python3.6 /usr/bin/python3.6m-config /usr/bin/python3.6m /usr/lib/python3 /usr/lib/python3.6 /usr/lib/python3.7 /etc/python3 /etc/python3.6 /usr/local/lib/python3.6 /usr/include/python3.6 /usr/include/python3.6m /usr/share/python3 /usr/share/man/man1/python3.1.gz

切换到路径下可执行对应文件


用创建“软链接”方法

ln -s /usr/bin/python3.5    /usr/bin/python3
       源文件                目标文件
```

- 环境变量路径分隔符
  
win7 用分号 ；  liunx 用冒号 :


## 爬虫常用库的安装

|类型|||
|---|---|----|
|请求库|requests selenium||
|解析库|beautifulsoup4 pyquery|
|存储库|pymysql pymongo redis|
|Web 库|flask django|
|网页notebook|jupyter|

>pip3 install requests selenium beautifulsoup4 pyquery pymysql pymongo redis flask django jupyter



# 爬虫基础

## 基本原理

- 什么是爬虫？

**请求**网站并**提取**数据的**自动化**程序


- 爬虫基本流程

|流程|内容|
|---|---|
|发起**请求**|通过 HTTP 库向目标站点发起请求，即发送一个 **Request** ,<br>请求可以包含额外的 headers 等信息，等待服务器响应。 |
|**获取**相应内容|如果服务器能正常响应，会得到一个 **Response**。<br>Response 的内容便是所获取的页面内容，类型可能有 HTML、Json 字符串，二进制数据等类型|
|**解析**内容|得到的内可能是HTML，可以用正则表达式、网页解析库进行解析。可能是Json，可以直接转化为 Json 对象解析，可能是二进制数据，可以做保存或者进一步处理。 |
|**保存**数据|保存形式多样，可以存为文本、数据库、指定格式文件。|

- Request

|Request|内容|
|---|---|
|请求方式|主要有GET、POST两种类型，另外还有HEAD、PUT、DELETE、OPTIONS等。|
|请求 URL|URL 统一资源定位符locator,如一个网页文档、一张图片、一个视频都可以用 URL 唯一来确定。|
|请求头|头部信息，如 User-Agent 、Host、Cookies 等信息|
|请求体|请求时额外携带的数据，如表单提交时的表单数据|


|Response|内容|
|---|---|
|响应状态|有多种响应状态，如 200代表成功、301跳转、404 找不到页面、502 服务器错误|
|响应头|如内容类型、内容长度、服务器信息、设置 Cookie 等。|
|响应体|最主要的部分，包含了请求资源的内容，如网页HTML、图片二进制数据等。|

- 能抓取怎样的内容

|能|抓取|怎样|的内容|
|---|---|---|---|
|网页文本|图片|音频|只要请求能得到的，都可以获取|

```

进入命令行交互模式

teenie@ubuntu:~$ python3

Python 3.6.8 (default, Jan 14 2019, 11:02:34) 
[GCC 8.0.1 20180414 (experimental) [trunk revision 259383]] on linux
Type "help", "copyright", "credits" or "license" for more information.

导入HTTP请求库，做模拟请求

>>> import requests

用get方法传入网址

>>> requests.get('http://www.baidu.com')

得到响应

<Response [200]>

拿到响应体的源代码

>>> requests.get('http://www.baidu.com').text
'<!DOCTYPE html>\r\n<!--STATUS OK--><html> <head><meta http-equiv=content-type content=text/html;charset=utf-8><meta http-equiv=X-UA-Compatible content=IE=Edge><meta content=always name=referrer><link rel=stylesheet type=text/css href=http://s1.bdstatic.com/r/www/cache/bdorz/baidu.min.css><title>ç\x99¾åº¦ä¸\x80ä¸\x8bï¼\x8cä½\xa0å°±ç\x9f¥é\x81\x93</title></head> <body link=#0000cc> <div id=wrapper> <div id=head> <div class=head_wrapper> <div class=s_form> <div class=s_form_wrapper> <div id=lg> <img hidefocus=true src=//www.baidu.com/img/bd_logo1.png width=270 height=129> </div> <form id=form name=f action=//www.baidu.com/s class=fm> <input type=hidden name=bdorz_come value=1> <input type=hidden name=ie value=utf-8> <input type=hidden name=f value=8> <input type=hidden name=rsv_bp value=1> <input type=hidden name=rsv_idx value=1> <input type=hidden name=tn value=baidu><span class="bg s_ipt_wr"><input id=kw name=wd class=s_ipt value maxlength=255 autocomplete=off autofocus></span><span class="bg s_btn_wr"><input type=submit id=su value=ç\x99¾åº¦ä¸\x80ä¸\x8b class="bg s_btn"></span> </form> </div> </div> <div id=u1> <a href=http://news.baidu.com name=tj_trnews class=mnav>æ\x96°é\x97»</a> <a href=http://www.hao123.com name=tj_trhao123 class=mnav>hao123</a> <a href=http://map.baidu.com name=tj_trmap class=mnav>å\x9c°å\x9b¾</a> <a href=http://v.baidu.com name=tj_trvideo class=mnav>è§\x86é¢\x91</a> <a href=http://tieba.baidu.com name=tj_trtieba class=mnav>è´´å\x90§</a> <noscript> <a href=http://www.baidu.com/bdorz/login.gif?login&amp;tpl=mn&amp;u=http%3A%2F%2Fwww.baidu.com%2f%3fbdorz_come%3d1 name=tj_login class=lb>ç\x99»å½\x95</a> </noscript> <script>document.write(\'<a href="http://www.baidu.com/bdorz/login.gif?login&tpl=mn&u=\'+ encodeURIComponent(window.location.href+ (window.location.search === "" ? "?" : "&")+ "bdorz_come=1")+ \'" name="tj_login" class="lb">ç\x99»å½\x95</a>\');</script> <a href=//www.baidu.com/more/ name=tj_briicon class=bri style="display: block;">æ\x9b´å¤\x9aäº§å\x93\x81</a> </div> </div> </div> <div id=ftCon> <div id=ftConw> <p id=lh> <a href=http://home.baidu.com>å\x85³äº\x8eç\x99¾åº¦</a> <a href=http://ir.baidu.com>About Baidu</a> </p> <p id=cp>&copy;2017&nbsp;Baidu&nbsp;<a href=http://www.baidu.com/duty/>ä½¿ç\x94¨ç\x99¾åº¦å\x89\x8då¿\x85è¯»</a>&nbsp; <a href=http://jianyi.baidu.com/ class=cp-feedback>æ\x84\x8fè§\x81å\x8f\x8dé¦\x88</a>&nbsp;äº¬ICPè¯\x81030173å\x8f·&nbsp; <img src=//www.baidu.com/img/gs.gif> </p> </div> </div> </div> </body> </html>\r\n'

```

- 怎样保存数据

|格式|保存数据|
|---|---|
|文本|**纯文本**、Json、Xml 等。|
|关系型数据库|如 MySQL、Oracle、SQL Server 等具有**结构化表结构**形式存储|
|非关系型数据库|如 MongoDB、Redis 等 **Key-Value** 形式存储|
|二进制文件|如图片、视频、音频等等直接保存成**特定格式**即可|

## 使用requests

- 安装

>root@ubuntu:/home/teenie# pip3 install requests

# 实战项目-抓取豆瓣某本书的评价



# 框架

# 分布式
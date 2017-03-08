---
layout: post
title:  "Apache屏蔽Ip及User-Agent"
date:   2017-03-08 23:27:00 +0800
categories: apache
---

今天网站突然负载很高，top发现cpu占用百分之八十以上，后来就无法访问，阿里云后台查看cpu已经百分百了，只好重启系统，然后查看http的日志发现是百度蜘蛛过度抓取页面导致整个网站无法访问，robots.txt里把所有蜘蛛屏蔽，发现没有作用，百度之后才发现百度蜘蛛貌似已经不再遵守robots.txt这个约定了，只能先把它给屏蔽掉了

##Ip屏蔽
首先需要搜出百度蜘蛛的Ip，它会后很多Ip，但是网段应该不会太多，用下面的命令找出其网段来
```
$ cat /var/log/httpd/access_log.20170308 | grep Baiduspider | cut -d. -f1,2,3 | sort | uniq
106.120.173
106.120.188
106.38.241
...
...

```
然后在apache配置文件里把网段全部屏蔽掉
```
Deny From 106.120.173
Deny From 106.120.188
Deny From 106.38.241
```

这样做之后百度蜘蛛的请求都403了，但是过一会发现apache日志里记录的百度蜘蛛的IP变成了这种格式: baiduspider-220-181-108-102.crawl.baidu.com, 这可能是百度蜘蛛检测到ip被屏蔽了的缘故，所以变幻了格式，最后决定屏蔽百度的user-agent

##User-Agent屏蔽

在apache配置文件里添加如下内容
```
BrowserMatch "Baiduspider" bad_bot
Order Allow,Deny
Allow from all
Deny From evn=bad_bot
```

一般蜘蛛的user-agent不会变，这样不管它是什么Ip来的，只要user-agent包含Baiduspider这个关键词就会被屏蔽掉，修改完之后重启apache立即生效，负载就降下来了

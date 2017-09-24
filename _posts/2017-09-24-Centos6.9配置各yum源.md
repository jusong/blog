---
layout: post
title:  "Centos6.9配置各yum源"
date:   2017-09-24 17:41:00 +0800
author: blacknc
categories: centos6 yum
---


### epel源

```
sudo yum localinstall --nogpgcheck http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
```

### remi源

```
sudo yum localinstall --nogpgcheck http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
```


### rpmfusion源

```
sudo yum localinstall --nogpgcheck http://download1.rpmfusion.org/free/el/updates/6/x86_64/rpmfusion-free-release-6-1.noarch.rpm

sudo yum localinstall --nogpgcheck http://download1.rpmfusion.org/nonfree/el/updates/6/x86_64/rpmfusion-nonfree-release-6-1.noarch.rpm
```

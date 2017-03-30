---
layout: post
title:  "php使用register_shutdown_function捕获E_ERROR致命错误"
date:   2017-01-02 02:28:47 +0800
author: blacknc
categories: php Fatal
---

#### E_ERROR 是为php脚本运行时发生的致命错误，一般为
        register_shutdown_function(array($this, 'shutdownHandler'));
		
        $errArr = error_get_last();

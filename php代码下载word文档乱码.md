<!--
author: blacknc
head: http://www.blacknc.com/img/2946691162925433.jpg
title: php代码下载word文档乱码 
tags: linux 编程 php 
category: 编程 
status: publish
summary: php代码下载word文档乱码 
-->

'
$file = fopen($fullFileName, "rb");
Header("Content-type: application/octet-stream");
Header("Accept-Ranges: bytes");
Header("Accept-Length: " . filesize($fullFileName));
Header("Content-Disposition: attachment; filename=" . $fileName);
ob_end_clean();     //添加此行，先清空输出缓冲区，再输出下载文件否则容易乱码
readfile($fullFileName);
'

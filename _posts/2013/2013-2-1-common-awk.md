---
layout: post
category: Linux
title: awk常用命令
---

1. 打印passwd文件每一行的全部内容(显示在终端)

	awk '{print $0}' /etc/passwd
	awk '{print }' /etc/passwd

1. 对于passwd文件的每一行，打印输出awk

	awk '{print "awk"}' /etc/passwd

1. 

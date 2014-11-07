---
layout: post
category: Programming
title: Chrome插件SwitchySharp源码学习
---

## 选项页options.html

在选项页options.html中，可以对该插件的一些参数进行设置，比如HTTP代理等。
该页中按钮onclick的响应函数在options.js中，且通过
chrome.extension.getBackgroundPage()获得背景页main.html，
而背景页main.html又调用了scripts及libs文件夹下的许多脚本。

this is a test line.

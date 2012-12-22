---
layout: post
category: Programming
title: Chrome插件SwitchySharp源码学习
---

作者：Hawstein

出处：<http://hawstein.com/posts/switchy-sharp-code-reading.html>

声明：本文采用以下协议进行授权：
[自由转载-非商用-非衍生-保持署名|Creative Commons BY-NC-ND 3.0](http://creativecommons.org/licenses/by-nc-nd/3.0/deed.zh)
，转载请注明作者及出处。

## 选项页options.html

在选项页options.html中，可以对该插件的一些参数进行设置，比如HTTP代理等。
该页中按钮onclick的响应函数在options.js中，且通过
chrome.extension.getBackgroundPage()获得背景页main.html，
而背景页main.html又调用了scripts及libs文件夹下的许多脚本。

---
layout: post
category: Linux
title: Linux Mint 12下的GLX问题
---

## 起因

逛GitHub的Explore，发现今日热门
Project:[Minecraft](https://github.com/fogleman/Minecraft)
用Python和Pyglet写的简单
[Minecraft](http://en.wikipedia.org/wiki/Minecraft)游戏，
代码只有几百行。觉得挺有意思的，于是fork过来，想学习一下。clone到本地运行，
问题开始出现。

## 经过

首先，运行`python main.py`后报错：

	pyglet requires an X server with GLX
	
Google之，追踪到Google group中的一个帖子，建议先运行glxinfo和glxgears
确保这两者运行是没有问题的。键入命令后发现，嗯，这是有问题的。

	$glxgears
	Xlib:  extension "GLX" missing on display ":0.0".
	Error: couldn't get an RGB, Double-buffered visual


	$glxinfo
	name of display: :0.0
	Xlib:  extension "GLX" missing on display ":0.0".
	Xlib:  extension "GLX" missing on display ":0.0".
	Xlib:  extension "GLX" missing on display ":0.0".
	Xlib:  extension "GLX" missing on display ":0.0".
	Xlib:  extension "GLX" missing on display ":0.0".
	Error: couldn't find RGB GLX visual or fbconfig

	Xlib:  extension "GLX" missing on display ":0.0".
	Xlib:  extension "GLX" missing on display ":0.0".
	Xlib:  extension "GLX" missing on display ":0.0".
	Xlib:  extension "GLX" missing on display ":0.0".
	Xlib:  extension "GLX" missing on display ":0.0".
	Xlib:  extension "GLX" missing on display ":0.0".
	Xlib:  extension "GLX" missing on display ":0.0".

然后追踪问题：

	Xlib: extension "GLX" missing on display ":0.0"
	
在[LinuxQuestions.org](http://www.linuxquestions.org/)
上看到个和我情况十分相似的问题，虽然那个帖子最终没有解决那个问题，
不过还是让我开始有一点眉目了。输入以下命令：

	cat /var/log/Xorg.0.log
	
找到出错的地方(EE):

	(EE) Failed to initialize GLX extension (Compatible NVIDIA X driver not found)

看到NVIDIA这几个字，我就大概知道什么问题了。
我的笔记本虽然是双显卡的，但使用Linux以来，独显一直都被我禁掉了(
在BIOS中禁用了，Acer的本，没禁用前CPU风扇响得全实验室都听得见XD)
既然N卡被我禁用了，其他程序什么的就不应该和它扯上关系。
然后继续追踪问题，输入以下命令：

	ldd /usr/bin/glxinfo
	
得到一条可能有问题的链接：

	libGL.so.1 => /usr/lib/nvidia-current/libGL.so.1
	
于是把nvidia-current中的文件全删除了，然后把libGL.so.1重新链接到另一个文件上。

	$ locate libGL.so
	/usr/lib/i386-linux-gnu/mesa/libGL.so.1
	/usr/lib/i386-linux-gnu/mesa/libGL.so.1.2
	/usr/lib/nvidia-current/libGL.so (nvidia的不理它)
	/usr/lib/nvidia-current/libGL.so.1
	/usr/lib/nvidia-current/libGL.so.280.13
	/usr/local/MATLAB/R2011b/sys/opengl/lib/glnx86/libGL.so.1 (MATLAB的不理它)
	/usr/local/MATLAB/R2011b/sys/opengl/lib/glnx86/libGL.so.1.5.070200

	$ sudo ln -s /usr/lib/i386-linux-gnu/mesa/libGL.so.1 /usr/lib/libGL.so.1

重启搞定。如果再输入`ldd /usr/bin/glxinfo`，libGL.so.1的链接是：

	libGL.so.1 => /usr/lib/libGL.so.1
	
glxinfo和glxgears两个命令也都没有问题了。

## 结果

Minecraft可以运行了，虽然这个游戏还非常简陋，不过我觉得还是值得学习的。

Note：生命在于折腾，折腾完之后最好记录一下你是怎么折腾的。

---
layout: post
category: Python
title: Pyglet教程
---

## 前言

用Python写程序很有趣，写游戏程序也很有趣。那么，用Python写游戏程序就更有趣了。
不幸的是，这方面的入门教程非常少。我想改变这种状况，所以我写了这个教程。

[pyglet programming guide](http://pyglet.org/doc/programming_guide/)
是非常好的资源，但如果你从来没写过游戏的话，它是帮不上你什么忙的。
这个教程将一步步教你写一个简单的小行星游戏Asteroids。本来对应这个教程，
还有一个大约40分钟的视频，但是呢，播放不了了，所以还是踏踏实实看文字版的吧。XD

## 目录

* [简介](#intro)
* [基本的图形](#graphic)
* [基本的运动](#motion)
* [让玩家有事可做](#do)
* [碰撞响应](#collision)
* [接下来做什么](#next)

## <a id="intro">简介</a>

### 这份文档是写给谁看的？

这份文档是写给那些懂得写简单Python程序的人看的，这是唯一的要求，没错，
你只需要懂得写简单的Python程序就可以看懂这份文档。如果你会写Python程序，
却发现这份文档有让你困惑不懂的地方，请Email我(steve+support@steveasleep.com),
我会修改这份文档。

### 为什么用Python写游戏

与你用Python来做其他事的原因一样：足够简单，有道理，且有许多库可以使用。

### 可用的库有哪些？

* [PyGame](http://www.pygame.org/)
* [Pyglet](http://www.pyglet.org/)
* [Panda3D](http://www.panda3d.org/)

### 我应该使用哪一个？

我个人的观点是，Pyglet是这三个里最轻便最快速的。当然，人们用PyGame
也做了许多很cool的东西，Panda3D比较复杂，主要面向3D，学习曲线更陡。
这个教程使用Pyglet来写游戏，但我不会劝说你放弃PyGame或是Panda3D而选择Pyglet，
爱用哪个库是你的自由。

为了使你熟悉Pyglet，我会一步步教你写一个简单版本的小行星游戏Asteroids。
如果这个过程中你遇到困难，可以通过阅读这个游戏不同阶段的版本来释惑。
<http://github.com/irskep/pyglettutorial>

## <a id="graphic">基本的图形</a>

Asteroids的第一个版本将简单地显示分数(0分)，程序的名字，三个随机放置的小行星，
和玩家的飞船。所有的东西都是静止不动的。

### 设置

**安装Pyglet**

从<http://pyglet.org/download.html>下载Pyglet并安装到你的电脑，
对于不同的平台，安装方法不一样，但都很简单，因为Pyglet没有额外的依赖包。

**文件设置**

由于我是分阶段来写这个例子的，所以我打算把放图片的文件夹和放程序的文件夹分开，
命名为resources。每一个版本的样例文件夹下，有一个asteroid.py文件来运行游戏，
同一目录下还有一个game模块，包含游戏大部分的功能。目录结构如下所示：

	mygame/
		resources/
			(images go here)
		version1/
			asteroids.py
			game/
				__init__.py

**显示窗口**

想要显示一个窗口，只需要简单地import pyglet，创建一个pyglet.window.Window
的实例，然后调用pyglet.app.run()即可。

{% highlight python %}
import pyglet
game_window = pyglet.window.Window(800, 600)

if __name__ == '__main__':
    pyglet.app.run()
{% endhighlight %}

当你运行上面的代码，将显示一个窗口，按Esc退出。

**加载并显示图片**

让我们创建一个game的子模块resources.py用来保存资源，

## <a id="motion">基本的运动</a>

## <a id="do">让玩家有事可做</a>

## <a id="collision">碰撞响应</a>

## <a id="next">接下来做什么</a>

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
由于图片所在的目录不在当前目录，因此我们需要告诉Pyglet去哪里找到它们：

{% highlight python %}
import pyglet
pyglet.resource.path = ['../resources']
pyglet.resource.reindex()
{% endhighlight %}

资源路径以"../"开头是因为resources文件夹与version1文件夹在同一目录下，
它表示要返回到父目录才能找到resources文件夹。如果我们去掉"../"，
pyglet就会在version1中查找resources文件夹(当然这样是查找不到的)。

pyglet的resources模块初始化后，就可以加载图片了。

{% highlight python %}
player_image = pyglet.resource.image("player.png")
bullet_image = pyglet.resource.image("bullet.png")
asteroid_image = pyglet.resource.image("asteroid.png")
{% endhighlight %}

**使图片居中**

pyglet默认是从左下角开始画图，但我们并不想这样，
于是我们通过设置图片的锚点使其居中。

{% highlight python %}
def center_image(image):
    """Sets an image's anchor point to its center"""
    image.anchor_x = image.width/2
    image.anchor_y = image.height/2
{% endhighlight %}

现在我们可以通过调用center_image()来使加载的图片居中：

{% highlight python %}
center_image(player_image)
center_image(bullet_image)
center_image(asteroid_image)
{% endhighlight %}

记住center_image()函数要先定义再调用，为了能在asteroids.py中访问图片，
我们需要使用类似"from game import resources"的语句，
这些内容将在下一节讲。

### 初始化物体

我们想在游戏窗口的顶部放置一些标签来显示当前游戏的分数与等级，
在第一个版本中，我们将做出一个能显示分数，游戏等级和代表生命数的图标的游戏。

**制作标签**

想要在pyglet中使用文字标签，只需要初始化一个pyglet.text.Label对象即可。

{% highlight python %}
score_label = pyglet.text.Label(text="Score: 0", x=10, y=575)
level_label = pyglet.text.Label(text="My Amazing Game", 
                                x=400, y=575, anchor_x='center')
{% endhighlight %}

注意第二个标签使用anchor_x属性进行居中处理。

**绘制标签**

我们希望pyglet调用我们定制的函数进行窗口绘制，为了达到这个目的，
我们有两种选择。第一种，继承pyglet中的Window类并重写on_draw()函数；
第二种，在一个相同名字的函数上使用@Window.event装饰器：

{% highlight python %}
@game_window.event
def on_draw():
    # draw things here
{% endhighlight %}

@game_window.event装饰器使得窗口实例知道on\_draw()函数是个事件句柄(event 
handler)。当窗口需要被重绘时，触发on\_draw事件。其它事件包括on\_mouse\_press,
on\_key\_press。

现在我们来完善这个函数让它来绘制标签。在我们进行绘制前，我们先清屏。清完屏后，
只需要简单地调用每个对象的draw()函数即可。

{% highlight python %}
@game_window.event
def on_draw():
    game_window.clear()

    level_label.draw()
    score_label.draw()
{% endhighlight %}

现在你运行asteroids.py，可以看到一个窗口，左上角显示0分，在顶部居中的地方写着：
“Version 1: Static Graphics”。

**飞船与小行星**

飞船应该是pyglet.sprite.Sprite的一个实例或是子类，比如：

{% highlight python %}
from game import resources
...
player_ship = pyglet.sprite.Sprite(img=resources.player_image, x=400, y=300)
{% endhighlight %}

只需要在on\_draw()函数中加一行，即可在窗口中绘制出飞船。

{% highlight python %}
@game_window.event
def on_draw():
    ...
    player_ship.draw()
{% endhighlight %}

加载小行星要稍微复杂一些，因为我们要随机地加载多个小行星在不同的位置上，
而且一开始还不能与飞船发生碰撞。我们把加载部分的代码放在一个新的模块，叫load.py。

{% highlight python %}
import pyglet, random
import resources

def asteroids(num_asteroids):
    asteroids = []
    for i in range(num_asteroids):
        asteroid_x = random.randint(0, 800)
        asteroid_y = random.randint(0, 600)
        new_asteroid = pyglet.sprite.Sprite(img=resources.asteroid_image, 
                                            x=asteroid_x, y=asteroid_y)
        new_asteroid.rotation = random.randint(0, 360)
        asteroids.append(new_asteroid)
    return asteroids
{% endhighlight %}

我们这里所做的就是在随机的位置上制造一些小行星。但这里还有一个问题，
由于小行星出现的位置是随机的，所以它有可能出现在飞船的位置。这样一来，
会导致飞船直接就挂掉。为了解决这个问题，
我们写一个简单的函数来计算小行星到飞船的距离。

{% highlight python %}
import math
...
def distance(point_1=(0, 0), point_2=(0, 0)):
    """Returns the distance between two points"""
    return math.sqrt((point_1[0]-point_2[0])**2+(point_1[1]-point_2[1])**2)
{% endhighlight %}

为了使新产生的小行星与飞船之间保持一定的距离，我们需要将飞船的位置传递给
asteroids()函数，然后不断地产生新的小行星坐标，直到它离飞船足够远。
pyglet sprites记录它们的位置有两种方法：元组(Sprite.position)和x,y属性，
(Sprite.x和Sprite.y)。为了保持代码简洁，我们将位置元组传递给函数。

{% highlight python %}
def asteroids(num_asteroids, player_position):
    asteroids = []
    for i in range(num_asteroids):
        asteroid_x, asteroid_y = player_position
        while distance((asteroid_x, asteroid_y), player_position) < 100:
            asteroid_x = random.randint(0, 800)
            asteroid_y = random.randint(0, 600)
        new_asteroid = pyglet.sprite.Sprite(img=resources.asteroid_image, 
                                            x=asteroid_x, y=asteroid_y)
        new_asteroid.rotation = random.randint(0, 360)
        asteroids.append(new_asteroid)
    return asteroids
{% endhighlight %}

对于每个小行星，不断地产生随机位置，直到这个位置离飞船比较远才生成它，
并给它一个随机的旋转角度。每个小行星都被加入到列表中返回。

现在你可以通过以下的方式加载3个小行星：

{% highlight python %}
from game import resources, load
...
asteroids = load.asteroids(3, player_ship.position)
{% endhighlight %}

变量asteroids是一个包含了若干个小行星的列表(文中是3个)，
绘制它们和绘制飞船同样容易，只需要调用它们的draw函数即可。

{% highlight python %}
@game_window.event
def on_draw():
    ...
    for asteroid in asteroids:
        asteroid.draw()
{% endhighlight %}

## <a id="motion">基本的运动</a>

在第二个版本的例子中，我们将介绍一种更简单更快速的方法来绘制物体。同时，
我们加一行图标来表示飞船剩余的生命数。我们还要写一些代码，
来使飞船和小行星遵循物理规律。

### 更多图形

**批量绘制**

如果我们有许多的东西要绘制，那么手工地调用每个物体的draw函数就会变得繁琐且乏味。
pyglet的批量绘制可以让你只通过一次简单的函数调用就将所有的东西绘制出来。
你所需要做的就是创建一个batch，将它传递给每一个你要绘制的物体，
然后调用batch的draw方法。

创建一个batch非常简单，代码如下：

{% highlight python %}
main_batch = pyglet.graphics.Batch()
{% endhighlight %}

为了使物体成为batch的一员，只需要将batch对象传递给物体的构造函数(使用关键字
batch)

{% highlight python %}
score_label = pyglet.text.Label(text="Score: 0", x=10, y=575, batch=main_batch)
{% endhighlight %}

我们要做的就是给每个需要绘制的物体的构建函数加一个batch关键字参数。

为了将小行星加入到batch中，我们需要将batch传递给game.load.asteroid()函数，
然后每创建一个小行星时只要加入这个关键字参数即可。

{% highlight python %}
def asteroids(num_asteroids, player_position, batch=None):
    ...
    new_asteroid = pyglet.sprite.Sprite(img=resources.asteroid_image, 
                                            x=asteroid_x, y=asteroid_y,
                                            batch=batch)
{% endhighlight %}

这样调用以上函数：
asteroids = load.asteroids(3, playership.position, main_batch)

现在我们可以用一行代码将所有的draw函数都替换掉：

{% highlight python %}
main_batch.draw()
{% endhighlight %}

你现在运行asteroids.py，效果和之前的是一模一样的。

**显示小飞船图标**

为了显示飞船剩余的生命数，我们将在窗口的右上角绘制一行小图标。
由于要使用相同的模板来绘制多个图标，我们在load模块中使用player_lives
函数来创建它们。

图标与飞船的图案是一样的，不过要小一些。我们可以事先做一个缩小的版本，
或者干脆就让pyglet来做这件事。我不知道你会怎么选，反正我怎么省事怎么来。

产生小图标的函数与产生小行星的函数基本是一样的，对于每个图标，
我们只需要创建一个sprite对象，给它一个位置及缩放尺度，然后加入到返回列表即可。

{% highlight python %}
def player_lives(num_icons, batch=None):
    player_lives = []
    for i in range(num_icons):
        new_sprite = pyglet.sprite.Sprite(img=resources.player_image, 
                                          x=785-i*30, y=585, 
                                          batch=batch)
        new_sprite.scale = 0.5
        player_lives.append(new_sprite)
    return player_lives
{% endhighlight %}

飞船大小是50*50的，所以小图标的大小是25*25。我们需要在两个图标间留一点空间，
于是我们从窗口的右端起，每隔30个像素绘制一个图标(这样图标间就有5个像素的间距)。
注意，就像asteroids()函数，player_lives()函数也有一个batch参数，
初始的None值表示默认是没有batch的。

### 让物体动起来


## <a id="do">让玩家有事可做</a>

## <a id="collision">碰撞响应</a>

## <a id="next">接下来做什么</a>

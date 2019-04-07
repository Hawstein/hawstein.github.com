---
layout: post
title: "Pyglet 教程"
author: "Hawstein"
header-style: text
tags:
  - Python
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

```python
import pyglet
game_window = pyglet.window.Window(800, 600)

if __name__ == '__main__':
    pyglet.app.run()
```

当你运行上面的代码，将显示一个窗口，按Esc退出。

**加载并显示图片**

让我们创建一个game的子模块resources.py用来保存资源，
由于图片所在的目录不在当前目录，因此我们需要告诉Pyglet去哪里找到它们：

```python
import pyglet
pyglet.resource.path = ['../resources']
pyglet.resource.reindex()
```

资源路径以"../"开头是因为resources文件夹与version1文件夹在同一目录下，
它表示要返回到父目录才能找到resources文件夹。如果我们去掉"../"，
pyglet就会在version1中查找resources文件夹(当然这样是查找不到的)。

pyglet的resources模块初始化后，就可以加载图片了。

```python
player_image = pyglet.resource.image("player.png")
bullet_image = pyglet.resource.image("bullet.png")
asteroid_image = pyglet.resource.image("asteroid.png")
```

**使图片居中**

pyglet默认是从左下角开始画图，但我们并不想这样，
于是我们通过设置图片的锚点使其居中。

```python
def center_image(image):
    """Sets an image's anchor point to its center"""
    image.anchor_x = image.width/2
    image.anchor_y = image.height/2
```

现在我们可以通过调用center_image()来使加载的图片居中：

```python
center_image(player_image)
center_image(bullet_image)
center_image(asteroid_image)
```

记住center_image()函数要先定义再调用，为了能在asteroids.py中访问图片，
我们需要使用类似"from game import resources"的语句，
这些内容将在下一节讲。

### 初始化物体

我们想在游戏窗口的顶部放置一些标签来显示当前游戏的分数与等级，
在第一个版本中，我们将做出一个能显示分数，游戏等级和代表生命数的图标的游戏。

**制作标签**

想要在pyglet中使用文字标签，只需要初始化一个pyglet.text.Label对象即可。

```python
score_label = pyglet.text.Label(text="Score: 0", x=10, y=575)
level_label = pyglet.text.Label(text="My Amazing Game", 
                                x=400, y=575, anchor_x='center')
```

注意第二个标签使用anchor_x属性进行居中处理。

**绘制标签**

我们希望pyglet调用我们定制的函数进行窗口绘制，为了达到这个目的，
我们有两种选择。第一种，继承pyglet中的Window类并重写on_draw()函数；
第二种，在一个相同名字的函数上使用@Window.event装饰器：

```python
@game_window.event
def on_draw():
    # draw things here
```

@game_window.event装饰器使得窗口实例知道on\_draw()函数是个事件句柄(event 
handler)。当窗口需要被重绘时，触发on\_draw事件。其它事件包括on\_mouse\_press,
on\_key\_press。

现在我们来完善这个函数让它来绘制标签。在我们进行绘制前，我们先清屏。清完屏后，
只需要简单地调用每个对象的draw()函数即可。

```python
@game_window.event
def on_draw():
    game_window.clear()

    level_label.draw()
    score_label.draw()
```

现在你运行asteroids.py，可以看到一个窗口，左上角显示0分，在顶部居中的地方写着：
“Version 1: Static Graphics”。

**飞船与小行星**

飞船应该是pyglet.sprite.Sprite的一个实例或是子类，比如：

```python
from game import resources
...
player_ship = pyglet.sprite.Sprite(img=resources.player_image, x=400, y=300)
```

只需要在on\_draw()函数中加一行，即可在窗口中绘制出飞船。

```python
@game_window.event
def on_draw():
    ...
    player_ship.draw()
```

加载小行星要稍微复杂一些，因为我们要随机地加载多个小行星在不同的位置上，
而且一开始还不能与飞船发生碰撞。我们把加载部分的代码放在一个新的模块，叫load.py。

```python
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
```

我们这里所做的就是在随机的位置上制造一些小行星。但这里还有一个问题，
由于小行星出现的位置是随机的，所以它有可能出现在飞船的位置。这样一来，
会导致飞船直接就挂掉。为了解决这个问题，
我们写一个简单的函数来计算小行星到飞船的距离。

```python
import math
...
def distance(point_1=(0, 0), point_2=(0, 0)):
    """Returns the distance between two points"""
    return math.sqrt((point_1[0]-point_2[0])**2+(point_1[1]-point_2[1])**2)
```

为了使新产生的小行星与飞船之间保持一定的距离，我们需要将飞船的位置传递给
asteroids()函数，然后不断地产生新的小行星坐标，直到它离飞船足够远。
pyglet sprites记录它们的位置有两种方法：元组(Sprite.position)和x,y属性，
(Sprite.x和Sprite.y)。为了保持代码简洁，我们将位置元组传递给函数。

```python
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
```

对于每个小行星，不断地产生随机位置，直到这个位置离飞船比较远才生成它，
并给它一个随机的旋转角度。每个小行星都被加入到列表中返回。

现在你可以通过以下的方式加载3个小行星：

```python
from game import resources, load
...
asteroids = load.asteroids(3, player_ship.position)
```

变量asteroids是一个包含了若干个小行星的列表(文中是3个)，
绘制它们和绘制飞船同样容易，只需要调用它们的draw函数即可。

```python
@game_window.event
def on_draw():
    ...
    for asteroid in asteroids:
        asteroid.draw()
```

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

```python
main_batch = pyglet.graphics.Batch()
```

为了使物体成为batch的一员，只需要将batch对象传递给物体的构造函数(使用关键字
batch)

```python
score_label = pyglet.text.Label(text="Score: 0", x=10, y=575, batch=main_batch)
```

我们要做的就是给每个需要绘制的物体的构造函数加一个batch关键字参数。

为了将小行星加入到batch中，我们需要将batch传递给game.load.asteroid()函数，
然后每创建一个小行星时只要加入这个关键字参数即可。

```python
def asteroids(num_asteroids, player_position, batch=None):
    ...
    new_asteroid = pyglet.sprite.Sprite(img=resources.asteroid_image, 
                                            x=asteroid_x, y=asteroid_y,
                                            batch=batch)
```

这样调用以上函数：
asteroids = load.asteroids(3, playership.position, main_batch)

现在我们可以用一行代码将所有的draw函数都替换掉：

```python
main_batch.draw()
```

你现在运行asteroids.py，效果和之前的是一模一样的。

**显示小飞船图标**

为了显示飞船剩余的生命数，我们将在窗口的右上角绘制一行小图标。
由于要使用相同的模板来绘制多个图标，我们在load模块中使用player_lives
函数来创建它们。

图标与飞船的图案是一样的，不过要小一些。我们可以事先做一个缩小的版本，
或者干脆就让pyglet来做这件事。我不知道你会怎么选，反正我怎么省事怎么来。

产生小图标的函数与产生小行星的函数基本是一样的，对于每个图标，
我们只需要创建一个sprite对象，给它一个位置及缩放尺度，然后加入到返回列表即可。

```python
def player_lives(num_icons, batch=None):
    player_lives = []
    for i in range(num_icons):
        new_sprite = pyglet.sprite.Sprite(img=resources.player_image, 
                                          x=785-i*30, y=585, 
                                          batch=batch)
        new_sprite.scale = 0.5
        player_lives.append(new_sprite)
    return player_lives
```

飞船大小是50\*50的，所以小图标的大小是25\*25。我们需要在两个图标间留一点空间，
于是我们从窗口的右端起，每隔30个像素绘制一个图标(这样图标间就有5个像素的间距)。
注意，就像asteroids()函数，player_lives()函数也有一个batch参数，
初始的None值表示默认是没有batch的。

### 让物体动起来

如果屏幕上的物体都不会动的话，那这个游戏显得就相当无趣了。为了使物体运动起来，
我们需要写一些类来处理每帧的运动。此处，我们还需要写一个Player类来响应键盘输入。

**创建基本运动类**

由于每个可视物体由一个Sprite对象表示，我们从pyglet.sprite.Sprite
中派生出基本运动类。另一种方法是让我们的类继承Object类并让他具有sprite属性，
但我觉得直接继承Sprite类会更方便。

我们创建一个新的子模块叫physicalobject.py并声明一个PhysicalObject类，
我们只加入一个新的属性：速度，这样一来，构造函数就相当简单了：

```python
class PhysicalObject(pyglet.sprite.Sprite):

    def __init__(self, *args, **kwargs):
        super(PhysicalObject, self).__init__(*args, **kwargs)
    
        self.velocity_x, self.velocity_y = 0.0, 0.0
```

每一帧中，每个物体都需要更新，因此我们需要一个update函数：

```python
def update(self, dt):
    self.x += self.velocity_x * dt
    self.y += self.velocity_y * dt
```

dt是时间间隔，游戏中帧间的过渡并不是瞬时的，而且它们也并不总是相等的时间间隔。
如果你曾经试过在一台老式机器上玩现代游戏，你会发现帧率变化很大(主要是低帧率吧)，
有许多方法可以解决这个问题，最简单的一个就是把所有时间敏感的操作都乘以dt。

如果给物体一个速度，让它们去运动，它们很快就会运动到屏幕外面。对于这款小行星游戏，
我们更希望的是它能从屏幕的另一侧出来，而不是消失。
用下面这个简单的函数就可以达到这个目的：

```python
def check_bounds(self):
    min_x = -self.image.width/2
    min_y = -self.image.height/2
    max_x = 800 + self.image.width/2
    max_y = 600 + self.image.height/2
    if self.x < min_x:
        self.x = max_x
    elif self.x > max_x:
        self.x = min_x
    if self.y < min_y:
        self.y = max_y
    elif self.y > max_y:
        self.y = min_y
```

正如你所见到的，它会检查物体在屏幕上是否仍然可见。如果物体从屏幕的一侧消失，
就让它从屏幕的另一侧出来。为了使每一个PhysicalObject对象都能遵循这样的法则，
我们在update函数的最后增加一个self.check_bounds()的调用。

为了使小行星能使用新的运动代码，只需要import physicalobject模块，
并改变"new_asteroid = ..."这一行，创建一个PhysicalObject对象，
而不是Sprite。此外，我们再给它一个随机的初始速度，以下是新的改进后的
load.asteroids()函数：

```python
def asteroids(num_asteroids, player_position, batch=None):
    ...
    new_asteroid = physicalobject.PhysicalObject(...)
    new_asteroid.rotation = random.randint(0, 360)
    new_asteroid.velocity_x = random.random()*40
    new_asteroid.velocity_y = random.random()*40
    ...
```

**游戏的update函数**

为了调用每一个物体的update函数，我们首先需要一个列表来存放这些物体。
现在我们只需要把所有物体都设置好后声明一下即可：

```python
game_objects = [player_ship] + asteroids
```

然后在这个列表上迭代一遍：

```python
def update(dt):
    for obj in game_objects:
        obj.update(dt)
```

**调用update函数**

物体的更新至少一帧一次。我们看到的视频大部分是每秒60帧，
但如果我们的程序也设定成这样的话，运动看起来就会有些不流畅。
因此我们将刷新频率设定为每秒钟120次，这样运动看起来就会平滑很多。

我们并不使用循环来更新每一帧，而是使用pyglet周期性地调用update函数，
pyglet.clock模块中有许多方法可以周期性地调用某个函数，
或是在未来指定时刻调用某个函数。这里我们使用pyglet.clock.schedule_interval():

	pyglet.clock.schedule_interval(update, 1/120.0)
	
把上面的代码放在if __name__ == '__main__' 块的pyglet.app.run()函数前，
告诉pyglet每秒去调用update函数120次。pyglet会传递经过的时间dt
作为唯一的参数给update函数。

现在如果你运行asteroids.py，你会发现之前静止的小行星已经动起来了，
而且当小行星从屏幕的一边消失后，将从屏幕的另一侧出来。

**编写Player类(即飞船类)**

除了遵循物理定律，飞船还需要能够响应键盘输入。我们通过继承PhysicalObject，
来编写我们的Player类：

```python
import physicalobject, resources

class Player(physicalobject.PhysicalObject):

    def __init__(self, *args, **kwargs):
        super(Player, self).__init__(img=resources.player_image, 
                                     *args, **kwargs)
```

到目前为止，Player和PhysicalObject唯一的区别就是Player总是使用相同的图片(
img=resources.player_image)。当然，Player还需要更多的属性。
不管飞船朝哪个方向运动，它总是使用相同的推力，因此我们需要定义一个推力常量：
thrust。同时我们还要定义飞船的旋转速度：

	self.thrust = 300.0
	self.rotate_speed = 200.0

现在我们需要让这个类来响应用户输入了。pyglet使用轮询方法(polling approach)
来处理键盘输入，发送键被按下与释放的消息来注册事件句柄(event handlers)。
我们需要经常去检查按键是否被按下，实现它的一个方法就是维护一个按键字典。
首先，我们要初始化这个字典：

	self.keys = dict(left=False, right=False, up=False)
	
接着我们需要写两个函数：`on_key_press()`和`on_key_release()`。
当pyglet检查一个新的事件句柄，它会调用这两个函数。

```python
import math
from pyglet.window import key
import physicalobject, resources
...
class Player(physicalobject.PhysicalObject)
    ...
    def on_key_press(self, symbol, modifiers):    
        if symbol == key.UP:
            self.keys['up'] = True
        elif symbol == key.LEFT:
            self.keys['left'] = True
        elif symbol == key.RIGHT:
            self.keys['right'] = True

    def on_key_release(self, symbol, modifiers):
        if symbol == key.UP:
            self.keys['up'] = False
        elif symbol == key.LEFT:
            self.keys['left'] = False
        elif symbol == key.RIGHT:
            self.keys['right'] = False
```

以上代码看起来相当不给力，后面我们会讲到更好的方法来实现同样的功能。不过，
对于现在这个版本来说，这样就OK了。

最后我们要做的是编写这个类的update函数，在PhysicalObject的update
函数的基础上，它还要增加一些代码。我们先调用父类PhysicalObject的
update函数，然后再去响应键盘输入。

```python
def update(self, dt):
	super(Player, self).update(dt)

	if self.keys['left']:
		self.rotation -= self.rotate_speed * dt
	if self.keys['right']:
		self.rotation += self.rotate_speed * dt
```

到目前为止都非常的简单，为了旋转飞船，我们减去或是加上旋转速度乘以dt。
注意这个旋转值的单位是角度，顺时针方向是正方向。这意味着你需要调用
math.degrees()或math.radians()来做角度和弧度的转换，
因为python内置的数学函数(比如sin，cos)使用的是弧度，
而且它们规定逆时针方向为正方向。下面的代码是让飞船向前推进运动：

```python
if self.keys['up']:
	angle_radians = -math.radians(self.rotation) #正方向定义不同，加负号
	force_x = math.cos(angle_radians) * self.thrust * dt
	force_y = math.sin(angle_radians) * self.thrust * dt
	self.velocity_x += force_x
	self.velocity_y += force_y
```

首先我们将角度值转化为弧度值，这样sin，cos才能接收到正确的参数值。
然后我们计算出飞船x，y方向的速度值。

**集成Player类**

首先我们需要创建一个Player的实例：player_ship：

```python
from game import player
...
player_ship = player.Player(x=400, y=300, batch=main_batch)
```

然后告诉pyglet player_ship是一个事件句柄(event handler)。
我们用game_window.push_handlers()函数把它压入事件栈中：

	game_window.push_handlers(player_ship)
	
OK，现在你可以运行游戏并用方向键来控制飞船了。

## <a id="do">让玩家有事可做</a>

在任何一款好游戏中，都要有与玩家对抗的东西。在小行星游戏中，
就是来自小行星撞击的威胁。碰撞检测需要写比较多的代码，
这一节我们只专注于如何让它工作起来。这一节中我们也将清理Player类，
并在飞船加速前进的时候显示一些视觉效果。

### 简化键盘输入

到目前为止，我们让Player类自己处理了所有的键盘事件。
我们写了13行代码来设置字典中的布尔变量，除此之外没做什么事情。
我们不禁会想有没有更好的办法来处理这些键的状态变化，答案是有，使用
pyglet.window.key.KeyStateHandler。这个类会自动地跟踪键盘上每个键的状态变化。

我们该如何使用它呢？首先初始化它并将它压入事件栈中。
我们需要将以下的代码加入Player类的构造函数：

	self.key_handler = key.KeyStateHandler()
	
然后将key_handler压入事件栈，

	game_window.push_handlers(player_ship.key_handler)

由于Player类现在使用key_handler来读取键盘状态，
我们需要改变update函数，需要改变的就只有if语句的条件：

	if self.key_handler[key.LEFT]:
		...
	if self.key_handler[key.RIGHT]:
		...
	if self.key_handler[key.UP]:
		...
		
这样一来，`on_key_press()`和`on_key_release()`就可以从这个类从移除了。
一切就是这么简单，如果你需要查看按键常量(比如key.LEFT，key.RIGHT等)，
可以查看pyglet.window.key下的API文档。

### 加入引擎火焰

没有视觉反馈的话，我们很难判断飞船是否在加速前进，尤其对于旁观者来说。
其中一个视觉反馈就是当飞船在向前推进的时候，在其后面加上引擎火焰(向后喷射的火焰)。

**加载火焰图片**

飞船现在由两个sprites构成，我们可以让一个Sprite拥有另一个Sprite，
因此我们给Player一个engine_sprite属性，并在每一帧中更新它。
这种方法是最简易并且扩展性最强的。

为了将火焰绘制在正确的位置上，我们可以选择在每一帧中做复杂的运算，
或者只是移动图片的锚点。不管怎样，我们先要在resources.py中加载图片：

	engine_image = pyglet.resource.image("engine_flame.png")

为了将火焰绘制在飞船尾部，我们要修改火焰图片的anchor_x 和anchor\_y属性：

	engine_image.anchor_x = engine_image.width * 1.5
	engine_image.anchor_y = engine_image.height / 2

现在火焰图片已经准备好给飞船使用了，如果你对锚点(anchor points)还有困惑的话，
可以多测试几组值来理解它。

**创建及绘制火焰**

引擎火焰的初始化与Player类的初始化一样(因为它们都是Sprite类)，
不同的是它所需要的图片不一样并且一开始它是不可见的。
我们在Player.__init__()中创建它：

	self.engine_sprite = pyglet.sprite.Sprite(img=resources.engine_image, 
                                          *args, **kwargs)
	self.engine_sprite.visible = False

为了使火焰只在飞船向前推进时显示，我们需要在update函数的
if self.key_handler[key.UP]语句下加一些代码：

```python
if self.key_handler[key.UP]:
	...
	self.engine_sprite.visible = True
else:
	self.engine_sprite.visible = False
```

为了使火焰总是出现在飞船尾部，我们需要及时更新它的位置及旋转属性：

```python
if self.key_handler[key.UP]:
	...
	self.engine_sprite.rotation = self.rotation
	self.engine_sprite.x = self.x
	self.engine_sprite.y = self.y
	self.engine_sprite.visible = True
else:
	self.engine_sprite.visible = False
```

**死亡后的清理工作**

当飞船被小行星撞毁，它就应该从屏幕上消失，我们可以调用Sprite的delete
函数来做这件事，但由于Player类有自己的Sprite对象(引擎火焰)，删除Player
类实例时也需要删除引擎火焰。因此我们把这两个删除工作放在一个delete函数中：

```python
def delete(self):
    self.engine_sprite.delete()
    super(Player, self).delete()
```

这样一来，Player类就清理完毕了。

### 碰撞检测

为了使物体从屏幕上消失，我们需要操作game_objects列表。
每个物体需要检查其它物体的位置与它的是否有冲突，然后决定是否从列表中移除它。
游戏循环将不断检测出死亡的物体并将它们从列表中移除。

**检查所有的物体对**

物体两两之间都要进行碰撞检查，最简单的方法就是使用双重循环。当物体数量很多时，
这种方法会很耗时，但对于我们的游戏来说是OK的(物体不多)。我们可以使用一点优化，
避免重复检查同一对物体。以下是update函数中的循环代码，
它迭代地取出所有的物体对，暂时什么事也不做：

```python
for i in xrange(len(game_objects)):
    for j in xrange(i+1, len(game_objects)):
        obj_1 = game_objects[i]
        obj_2 = game_objects[j]
```

我们需要某种方法来检测一个物体是否已经被干掉了，现在我们先不去理它，
而专注于当前的循环。假设game\_objects中的物体都有一个死亡属性并且初始化为false，
当它被设为true时，表示物体已经死亡，可以从列表中移除。

我们还需要另外两个方法来处理碰撞：一个方法判断两个物体是否发生碰撞，
另一个方法是让物体去处理碰撞。直接看下面代码，很容易理解：

```python
if not obj_1.dead and not obj_2.dead:
	if obj_1.collides_with(obj_2):
		obj_1.handle_collision_with(obj_2)
		obj_2.handle_collision_with(obj_1)
```

接下来就只需要将列表中的死亡物体移除即可。

```python
...update game objects...

for to_remove in [obj for obj in game_objects if obj.dead]: 
	to_remove.delete() 
	game_objects.remove(to_remove)
```

正如你所看到的，物体调用delete方法将它从任一batches中移除，
然后从列表中移除该物体。上述代码中，中括号里表示的是列表推导式(list 
comprehensions)，它将game\_objects列表中已经死亡的物体拿出来形成一个新的列表。

**实现碰撞函数**

我们需要往PhysicalObject类里加3个东西：dead属性，collides\_with()方法和
handle\_collision\_with()方法。collides\_with()方法需要用到distance()
函数，因此我们先将该函数放到game中的一个子模块，命名为util.py：

```python
import pyglet, math 
def distance(point_1=(0, 0), point_2=(0, 0)): 
	return math.sqrt(
		(point_1[0] - point_2[0]) ** 2 + 
		(point_1[1] - point_2[1]) ** 2)
```

记得要在load.py中调用`from util import distance`，现在我们可以来完成
collides_with()函数了：

```python
def collides_with(self, other_object): 
	collision_distance = self.image.width/2 + other_object.image.width/2 
	actual_distance = util.distance( 
		self.position, other_object.position) 
	return (actual_distance <= collision_distance)
```

碰撞处理函数就更加简单了，
因为目前我们想要的只是一个物体在撞到另一个物体时立即死亡。

```python
def handle_collision_with(self, other_object): 
	self.dead = True
```

最后一件事，将物体的死亡属性在`PhysicalObject.__init__()`设为False。

That's it!现在你应该可以让你的飞船在屏幕上喷着火焰飞来飞去。如果飞船撞上一个东西，
飞船和那个东西都会从屏幕上消失。这还称不上是一个游戏，但明显我们一直在进步着。

## <a id="collision">碰撞响应</a>

在这一节中，我们将加入子弹。这项新特性要求我们在游戏的过程中需要向game\_objects
里加入东西，与此同时，我们需要去判断相撞物体的类别来决定他们是否应该死亡。

### 在游戏过程中加入物体

**How**

我们用一个布尔变量来处理物体的移除问题，加入物体就没那么简单了。一方面，
一个物体不能简单地喊一句：嘿，把我回到列表中。它需要来自某个地方。另一方面，
一个物体可能需要一次加入多个物体。

我们有不同的方法可以来解决这个问题，这里选择一个简单的方案，
让每一个物体维护一个列表，这个列表用来保存它的子对象(这里是子弹)。
这种方法可以非常简单地在游戏中向每个物体加入子对象。

**对游戏循环做微调**

一种简单的思路是每次检查物体的子对象并将它的子对象加到game\_objects列表中。
如下所示：(只需加两行代码，其中new\_objects是一个子对象列表)

```python
for obj in game_objects: 
	obj.update(dt) 
	game_objects.extend(obj.new_objects) 
	obj.new_objects = []
```

不幸的是，上面的做法是有问题的。我们本意是想从game\_objects
列表中迭代地取出元素进行操作，可是我们却在函数体中改变了这个列表。当然了，
这个问题很容易解决，我们只需要将新的对象添加到另一个列表，然后在这个for
循环结束后再将这个列表添加到game\_objects列表即可。看代码：

```python
...collision...

to_add = []

for obj in game_objects:
    obj.update(dt)
    to_add.extend(obj.new_objects)
    obj.new_objects = []

...removal...

game_objects.extend(to_add)
```

**在PhysicalObject类中加入新属性**

在上面的代码中，我们用到了new\_objects，因此我们需要在PhysicalObject
中添加一下：

```python
def __init__(self, *args, **kwargs):
    ....
    self.new_objects = []
```

如果要加入新物体，我们所需要做的就是将它添加到new\_objects。
然后在主循环中它会被添加到game\_objects列表，而new\_objects会被清空。

### 加入子弹

**编写子弹类**

大部分时候，子弹与其他PhysicalObject类的表现没有什么区别。
不过在这个游戏中，至少有两点是不一样的：1.子弹只与部分物体发生碰撞(只与小行星)，
2.子弹会在一定的时间内消息，否则的话如果它不碰上小行星将会导致满屏都是子弹。

首先，我们在game下创建一个子模块叫bullet.py，将Bullet作为PhysicalObject
的子类：

```python
import pyglet
import physicalobject, resources

class Bullet(physicalobject.PhysicalObject):
    """Bullets fired by the player"""

    def __init__(self, *args, **kwargs):
        super(Bullet, self).__init__(
            resources.bullet_image, *args, **kwargs)
```

为了使子弹在一段时间之后从屏幕消失，我们可以维护子弹的当前年龄及寿命属性，
或者让pyglet来帮我们做这些事。我不知道你们怎么想的，反正我是喜欢第二种方案。
首先我们定义一个函数在子弹消亡时调用：

```python
def die(self, dt):
    self.dead = True
```

现在我们要告诉pyglet在子弹创建后大约0.5秒调用上面的函数，
我们可以在构造函数中加入pyglet.clock.schedule_once()来实现：

```python
def __init__(self, *args, **kwargs):
    super(Bullet, self).__init__(
        resources.bullet_image, *args, **kwargs)
    pyglet.clock.schedule_once(self.die, 0.5)
```

Bullet类还有许多地方需要完善，但在些之前，我们先把子弹呈现到屏幕上。无图无真相，
对吧。

**让子弹飞**

Player类是唯一需要子弹的类，因此打开这个文件，往里面import bullet模块，
并在构造函数中添加子弹速度bullet\_speed：

```python
...
import bullet

class Player(physicalobject.PhysicalObject):
    def __init__(self, *args, **kwargs):
        super(Player, self).__init__(
            img=resources.player_image, *args, **kwargs)
        ...
        self.bullet_speed = 700.0
```

现在我们可以写代码来生成子弹并把它发射出去了。首先，我们需要定义`on_key_press`
事件处理程序：

```python
def on_key_press(self, symbol, modifiers):
    if symbol == key.SPACE:
        self.fire()
```

发射子弹的函数fire()要稍微复杂一些。大部分的计算与上文推力的处理相似，
不过还是有一些不同的地方。比如，我们要让子弹从飞船的头部发射出去而非飞船中心；
此外，我们还需要将飞船的当前速度加到子弹的初始速度上，
否则当飞船运行过快时将超过子弹，显然这不是我们想看到的。

一开始我们要将角度转换成弧度并逆转方向：

```python
def fire(self):
    angle_radians = -math.radians(self.rotation)
```

接着，计算子弹的位置并实例化它：

```python
ship_radius = self.image.width/2
    bullet_x = self.x + math.cos(angle_radians) * ship_radius
    bullet_y = self.y + math.sin(angle_radians) * ship_radius
    new_bullet = bullet.Bullet(bullet_x, bullet_y, batch=self.batch)
```

子弹速度的计算与飞船速度计算类似：

```python
 bullet_vx = (
        self.velocity_x +
        math.cos(angle_radians) * self.bullet_speed
    )
    bullet_vy = (
        self.velocity_y +
        math.sin(angle_radians) * self.bullet_speed
    )
    new_bullet.velocity_x = bullet_vx
    new_bullet.velocity_y = bullet_vy
```

最后把子弹加到`new_objects`列表，这样主循环就会把它加到`game_objects`中。

```python
self.new_objects.append(new_bullet)
```

到了这一步，你应该可以在你的飞船头部发射子弹了。不过你会发现一个问题，
当你发射子弹时，你的飞船就消失了。不仅如此，你会发现当两个小行星碰撞时，
它们也会消失。这些都是因为我们在前面的代码中设置了，当两个物体发生碰撞时，
它们就会消失。为了解决这个问题，我们需要去定制每个类的`handle_collision_with`
方法。

### 定制碰撞后的行为

在当前版本的游戏中，存在5类碰撞：子弹-小行星，子弹-飞船，子弹-子弹，小行星-飞船，
小行星-小行星。更复杂的游戏将存在更多样的碰撞。

一般来说，相同物体发生碰撞不应该销毁，我们要在PhysicalObject类中实现这个行为。
其他类型的碰撞需要稍微多做一些工作。

**忽略同类间的碰撞**

如果两个小行星或两颗子弹发生碰撞，直接忽略，让它们沿原来的轨迹运行，什么也不做。
我们只需要在`PhysicalObject.handle_collision_with()`方法中加入类型判断：

```python
def handle_collision_with(self, other_object):
    if other_object.__class__ == self.__class__:
        self.dead = False
    else:
        self.dead = True
```

上述代码也可以使用`type(self) == type(other_object)`
来判断两个物体是否属于同一类。

**定制子弹的碰撞**

由于子弹对于不同物体的碰撞会有不同的反应，我们向PhysicalObjects
添加`reacts_to_bullets`属性，这个属性表明是否需要对子弹做出响应(飞船不用响应)
。此外我们可以再添加一个`is_bullet`属性来表示一个物体是否是子弹。

(这些设计并不怎么好，不过他们可以work)

首先，在PhysicalObject的构造函数里把`reacts_to_bullets`设为True：

```python
class PhysicalObject(pyglet.sprite.Sprite):
    def __init__(self, *args, **kwargs):
        ...
        self.reacts_to_bullets = True
        self.is_bullet = False
        ...

class Bullet(physicalobject.PhysicalObject):
    def __init__(self, *args, **kwargs):
        ...
        self.is_bullet = True
```

然后，在`PhysicalObject.collides_with()`函数中添加一些代码，
使其在适当的情况下忽略子弹：

```python
def collides_with(self, other_object):
        if not self.reacts_to_bullets and other_object.is_bullet:
            return False
        if self.is_bullet and not other_object.reacts_to_bullets:
            return False
        ...
```

最后，在`Player.__init__()`中设置`self.reacts_to_bullets = False`。
这样一来，Bullet类就完成了！现在，我们需要决定当子弹击中小行星时，
要发生些什么。

### 令小行星爆炸

为了使游戏更好玩一些，我们采用这样的思路：当你击中小行星时，它会变成更多的，
更小的小行星，这相当于把游戏难度加大了。当然了，当然了这种变化要控制在有限次，
否则小行星只会越打越多。该游戏把它限制为2次。假如初始小行星大小为Large，
击中后则会变化若干个大小为Middle的小行星，击中Middle的小行星的话，
则会变成若干个大小为Small的小行星，击中大小为Small的小行星则小行星死亡。

接下来，我们要实现一个小行星类Asteroid并定制它的`handle_collision_with()`
方法。

**写一个小行星类**

在game文件夹下创建一个新的子模块叫`asteroid.py`，
在构造函数中把小行星的图片传给它的超类。

```python
import pyglet
import resources, physicalobject

class Asteroid(physicalobject.PhysicalObject):
    def __init__(self, *args, **kwargs):
        super(Asteroid, self).__init__(
            resources.asteroid_image, *args, **kwargs)
```

现在我们需要写一个新的`handle_collision_with()`方法，使得小行星被击中时，
会产生随机数目的、随机速度的更小的小行星。而且一个小行星最多变小2次，
即从Large到Middle再到Small，每次变为原来大小的1/2。

我们要忽略两个小行星间的碰撞，这一情况可以调用它超类的方法来处理：

```python
 def handle_collision_with(self, other_object):
        super(Asteroid, self).handle_collision_with(other_object)
```

当小行星被击中而变成更小的小行星时，我们要把大的小行星的速度加到小的小行星上，
使其看起来是来自原来的小行星。

```python
import random
...
class Asteroid...
    def handle_collision_with(self, other_object):
        super(Asteroid, self).handle_collision_with(other_object)
        if self.dead and self.scale > 0.25:
            num_asteroids = random.randint(2, 3) #产生2个或3个小行星
            for i in xrange(num_asteroids):
                new_asteroid = Asteroid(
                    x=self.x, y=self.y, batch=self.batch)
                new_asteroid.rotation = random.randint(0, 360)
                new_asteroid.velocity_x = (
                    random.random() * 70 + self.velocity_x)
                new_asteroid.velocity_y = (
                    random.random() * 70 + self.velocity_y)
                new_asteroid.scale = self.scale * 0.5
                self.new_objects.append(new_asteroid)
```

我们可以为每个小行星加上些旋转使得画面更有动感。为此，我们需要定义一个旋转速度
`rotate_speed`并给它一个随机的值。然后写一个`update()`将旋转应用到第一帧。

在构造函数中加入`rotate_speed`：

```python
 def __init__(self, *args, **kwargs):
        super(Asteroid, self).__init__(
            resources.asteroid_image, *args, **kwargs)
        self.rotate_speed = random.random() * 100.0 - 50.0
```

`update()`函数：

```python
def update(self, dt):
        super(Asteroid, self).update(dt)
        self.rotation += self.rotate_speed * dt
```

最后一件事，在`load.py`的`asteroids()`方法中创建`Asteroid`对象，
而不是`PhysicalObject`对象。

```python
import asteroid

def asteroids(num_asteroids, player_position, batch=None):
    ...
    for i in range(num_asteroids):
        ...
        new_asteroid = asteroid.Asteroid(
            x=asteroid_x, y=asteroid_y, batch=batch)
        ...
    return asteroids
```

## <a id="next">接下来做什么</a>

教程到这里就结束了，接下来你可以自己去扩展，或者去读源码：
<http://github.com/irskep/pyglettutorial>。我不再讲解下去，原因：

1. 如果你自己不动手，你是不会学到更多的
1. 我已经筋疲力尽了
1. 你不需要我了

因此接下来就靠你自己去发挥了，以下是一些练习：

1. 实现计分器
1. 如果game over了，可以让玩家从头开始
1. 实现生命计数及"Game Over"字幕
1. 加入粒子效果(使用[Lepton](http://code.google.com/p/py-lepton/)或你自己的粒子引擎)

Good luck!

原文链接：<http://steveasleep.com/pyglettutorial.html>

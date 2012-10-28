---
layout: post
category: Markdown
title: Markdown的常用语法及Emacs下的快捷键
---
## Markdown的常用语法

建议阅读以下文章，讲得挺详细的。

* 原文：[Markdown: Syntax](http://daringfireball.net/projects/markdown/syntax)
* 译文：[Markdown语法](http://www.ituring.com.cn/article/775)

当然了，我可不想每次忘记了一个什么语法，又专门去上面的文章里找，也挺麻烦的。
所以列出一些常用的：

- `*`和`_`包裹的文本表示强调该内容,如：`*强调*`

- `**`和`__`包裹的文本表示加粗该内容，对应HTML的\<strong\>，如：`**加粗**`

- 用空格包裹`*`和`_`，它就失去强调含义，成为字面上的星号或下划线

- 不想让Markdown解析字符就用`\`转义它

- 行内代码用反引号`` ` ``包住它

- \# 一级标题

- \#\# 二级标题

- \#\#\# 三级标题（最多可到6级）

- \> 引用

- */+/- 无序列表(它们是等价的)

- 使用数字就是有序列表，无论是什么数字。所以你会钟爱在每个列表项前加个1. 的

- 列表项中使用引用(>)，需要缩进

- 缩进四个空格或者一个TAB生成代码块

- 代码块中&,<,>会自动转换为HTML实体

- Markdown不会解析代码块和行内代码中的Markdown标记

- 在一行里放三个或更多的`*`或`-`会得到一条水平线

- 链接：`[链接文字](链接地址)`如`[an example](http://example.com/)`

- 引用本地资源，使用相对路径：`[about me](/about/)`

- 行内图片：`![Alt文字](/path/to/img.jpg "Optional title")`，也可以用\<img\>标签

- 自动链接。产生一个可点击的链接：`<http://example.com/>`

## Emacs的Markdown模式

参考链接：[Emacs Markdown模式简介](http://linuxtoy.org/archives/emacs-markdown-mode.html)

### 编辑命令

Markdown 模式将常用的编辑命令都绑定到了特定的组合键上，因此要插入某个项目，只需按相应的组合键。比如：

- C-c C-t n 插入 hash 样式的标题，其中 n 为 1~5，表示从第一级标题到第五级标题。
- C-c C-t t 插入 underline 样式的标题，这是一级。
- C-c C-t s 同上，这是二级。
- C-c C-a l 插入链接，格式为 \[text\](url)。
- C-c C-i i 插入图像，格式为 \!\[text\](url)。
- **C-c C-s b 插入引用内容。**
- **C-c C-s c 插入代码。**
- **C-c C-p b 加粗。**
- C-c C-p i 斜体。
- **C-c - 插入水平线。**

如果是在选定的内容上按这些组合键，那么将把选定的内容设为相应的格式。
加粗的是最常用的。

---
layout: post
category: Programming
title: Google C++编程风格指南
---

## 前言

越来越发现一致的编程风格的重要性，于是把Google的C++编程风格指南看了一遍，
这里记录下于自己有益的rules。当规则有多个选择时，这里只记录个人习惯的用法，
并不代表它是唯一的用法。

[Google Style Guide](http://code.google.com/p/google-styleguide/)

[Google开源项目风格指南](https://github.com/brantyoung/zh-google-styleguide/)

## 命名约定

命名管理是最重要的一致性规则，因此我把它放在最前面。

1. 尽可能给出描述性名称。如：

{% highlight cpp %}
int num_errors; 
int num_completed_connections;
{% endhighlight %}

1. 文件名全部小写，用下划线做连接符。

{% highlight cpp %}
my_useful_class.cc
{% endhighlight %}

1. C++文件以.cc 结尾，头文件以.h 结尾。(从.cpp切换到.cc)

1. 类型命名每个单词以大写字母开头，不包含下划线。类、结构体、类型定义(typedef)
、枚举都使用相同约定。

{% highlight cpp %}
// classes and structs
class UrlTable { ...
class UrlTableTester { ...
struct UrlTableProperties { ...

// typedefs
typedef hash_map<UrlTableProperties *, string> PropertiesMap;

// enums
enum UrlTableErrors { ...
{% endhighlight %}

1. 变量名一律小写，单词之间用下划线连接。类的成员变量以下划线结尾。

{% highlight cpp %}
my_exciting_local_variable
my_exciting_member_variable_
{% endhighlight %}

1. 结构体的数据成员可以和普通变量一样，不用像类那样接下划线。

{% highlight cpp %}
struct UrlTableProperties {
	string name;
	int num_entries;
}
{% endhighlight %}

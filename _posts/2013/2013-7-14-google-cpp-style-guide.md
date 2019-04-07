---
layout: post
title: "Google C++ 编程风格指南"
author: "Hawstein"
header-style: text
tags:
  - C++
---

## 前言

越来越发现一致的编程风格的重要性，于是把Google的C++编程风格指南看了一遍，
这里记录下于自己有益的rules。当规则有多个选择时，这里只记录个人习惯的用法，
并不代表它是唯一的用法。

[Google Style Guide](http://code.google.com/p/google-styleguide/)

[Google开源项目风格指南](https://github.com/brantyoung/zh-google-styleguide/)

## 命名约定

命名管理是最重要的一致性规则，因此我把它放在最前面。

* 尽可能给出描述性名称。

```cpp
int num_errors; 
int num_completed_connections;
```

* 文件名全部小写，用下划线做连接符。

```cpp
my_useful_class.cc
```

* C++文件以.cc 结尾，头文件以.h 结尾。(从.cpp切换到.cc)

```cpp
my_useful_class.cc
my_useful_class.h
```

* 类型命名每个单词以大写字母开头，不包含下划线。类、结构体、类型定义(typedef)
、枚举都使用相同约定。

```cpp
// classes and structs
class UrlTable { ...
class UrlTableTester { ...
struct UrlTableProperties { ...

// typedefs
typedef hash_map<UrlTableProperties *, string> PropertiesMap;

// enums
enum UrlTableErrors { ...
```

* 变量名一律小写，单词之间用下划线连接。类的成员变量以下划线结尾。

```cpp
my_exciting_local_variable
my_exciting_member_variable_
```

* 结构体的数据成员可以和普通变量一样，不用像类那样接下划线。

```cpp
struct UrlTableProperties {
	string name;
	int num_entries;
}
```

* 少用全局变量，要用的话用g作为其前缀(不喜欢用g_)。

```cpp
bool gInvalid = false;
```

* 常量命名在名称前加k。

```cpp
const int kDaysInAWeek = 7;
```

* 函数名的每个单词首字母大写，没有下划线。

```cpp
AddTableEntry()
DeleteUrl()
```

* 取值和设值函数要与存取的变量名匹配，使用小写单词及下划线。

```cpp
class MyClass {
public:
    ...
    int num_entries() const { return num_entries_; }
    void set_num_entries(int num_entries) { num_entries_ = num_entries; }

private:
    int num_entries_;
};
```

* 非常短小的内联函数也可以用小写字母命名。

```cpp
void swap(int &a, int &b);
int max(int a, int b);
bool cmp(Type t1, Type t2);
```

* 名字空间用小写字母命名，并基于项目名称和目录结构：

```cpp
namespace google_awesome_project {
	...
}
```

* 枚举值应该优先采用常量的命名方式。

```cpp
enum UrlTableErrors {
    kOK = 0,
    kErrorOutOfMemory,
    kErrorMalformedInput,
};
```

* 尽量避免使用宏，如果不得不用，请使用大写字母及下划线。

```cpp
#define ROUND(x) ...
#define PI_ROUNDED 3.0
```


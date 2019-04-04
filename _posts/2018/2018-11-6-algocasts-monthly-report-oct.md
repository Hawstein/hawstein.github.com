---
layout: post
category: AlgoCasts
title: AlgoCasts 10 月小结
---

不知不觉，[AlgoCasts](https://algocasts.io) 已经上线一个月了。这一个月做了不少事情，收获满满。

<img width="250px" src="/assets/img/2018/11/6/applaud.jpg" />

AlgoCasts 是在 9 月 25 号的下午正式上线的，在此之前就是默默地写代码和录制第一批视频。上线第二天开始有用户付费，这一点大大出乎了我的意料。我现在还记得收到第一笔付款时的激动心情：）而且有意思的是，第一位付费用户是我之前博客的订阅者，也就是说，他是在我博客断更两年后（囧）突然收到了篇莫名其妙的文章更新，然后跑来付费支持的，无论如何，要给他一个大大的感谢。

接下来的每一天，都有用户从不同的地方来到 AlgoCasts 网站，每天的付费用户从 1～4 个不等，而且基本全买了 Plan 100 这个套餐（后面考虑把 Plan 40 下掉），借这个机会再次向你们致谢。

<img width="250px" src="/assets/img/2018/11/6/thanks.jpg" />

过去一个月，我每天都在制作和更新视频，并且会在[微博](https://weibo.com/hawsteinstudio)、[Twitter](https://twitter.com/hawsteinstudio) 和微信（Hawstein-Studio）上发布更新信息，目前视频已经更新到

<img width="100px" src="/assets/img/2018/11/6/80.png" /> 个（[戳我看视频列表](https://algocasts.io/episodes)）。

眼瞅着 Plan 100 就要录制完了，趁着现在还是早鸟价，少年，你是不是该入手了？

<img width="400px" src="/assets/img/2018/11/6/shutup.jpg" />

目前对我来说，最重要的就是制作更多更优质的视频，而且视频做起来还是非常耗时耗精力的，所以没有花多少时间去给网站加各种功能。过去一个月除了制作视频，主要做了以下一些微小的工作。

* 视频标记学习功能（感谢会员 @T 的建议）
* 静态资源上 CDN（由于网站没备案，用了 jsdelivr）
* VPS 从东京换到香港，CN2 线路，确保大陆访问速度
* 使用 Ansible 初步完成运维自动化项目
* 数据库定时备份
* 上线 Slack 机器人，方便网站管理

其中花了比较多时间的是，开始用 Ansible 来做一些运维自动化的事情。无论是我自己的项目，还是和其他小伙伴搞的一些项目，已经遇到过好几次的服务器迁移了。迁移服务器时，基础环境/安全/监控/数据库等等一系列的事情都是一些繁琐/费时/重复的工作，在手动撸过几次后，实在不想后面再做重复劳动了，于是这次抵制住了登录服务器一把梭的念头，一点点把初步的运维自动化项目写起来。后面在运维服务时再慢慢向这个项目添砖加瓦。

Ansible 项目写完后，服务迁移就 So easy 了。下面是迁移前后的 Ping 值对比。不知道小伙伴们最近访问网站有没感受到速度上的变化。

迁移前后的 Ping 值对比：

<img width="400px" src="/assets/img/2018/11/6/ping-linode.png" />    VS
<img width="400px" src="/assets/img/2018/11/6/ping-ufo.png" />

这段时间还收到一些表扬与认可，感谢大家。我会继续加油的：）

<img width="400px" src="/assets/img/2018/11/6/praise.png" />

11 月，继续好好录制视频，并且抽空把大家的一些好建议实现了。

<img width="250px" src="/assets/img/2018/11/6/duck.jpg" />

如果有任何疑问，或者只是想和我聊聊，都可以扫码加微信。

<img width="250px" src="/assets/img/2018/11/6/hawstein-studio-wechat.jpeg" />

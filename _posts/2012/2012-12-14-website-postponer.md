---
layout: post
category: Programming
title: 限制你上某些网站的Chrome插件——Website Postponer
---

作者：Hawstein

出处：<http://hawstein.com/posts/website-postponer.html>

声明：本文采用以下协议进行授权：
[自由转载-非商用-非衍生-保持署名|Creative Commons BY-NC-ND 3.0](http://creativecommons.org/licenses/by-nc-nd/3.0/deed.zh)
，转载请注明作者及出处。

## 前言

写这样一个插件的想法是在某一天睡觉的时候产生的。为的是解决生活中遇到的一个问题——
刷微博(目测还有人人网)。我一天大部分的时间都在实验室里，看论文，写程序，做项目，
作为一个CSer，终日与电脑为伴。有时候，我回顾过去的一天，会发现有许多的时间都浪费
在漫无目的的网页浏览上了，尤其是在微博和人人上。虽然每次上微博或是人人就是看一下，
但一天频繁地登录，其实在上面还是浪费了很多时间。最关键的是，把我的工作时间分割得
支离破碎，大大降低了工作效率。

对我来说，基本上所有SNS的用户黏性都不大。但我在实验室里还是会时不时地登录一下，
这是为什么呢？原因很简单，人的天性都是倾向于向抵抗力最小的方向走的。
当你正在看一篇很难懂的Paper，在写一个很难的程序，或是有一个BUG，你找了半天也
没找到，又或是正在做的某个项目功能没头绪了。这时候就想先放一放，然后就很自然地打
开了微博和人人，在上面随便看一看，瞧一瞧。然后，欢乐的时光总是过得很快，不知不觉
地，你发现你在上面已经看了一个小时了！有时候，你也许只是想，嗯，刚吃完饭嘛，
休息一下，看看微博和人人，结果又是一个不小心，在上面花了很多的时间。
网上的信息量非常巨大，如果你是漫无目的地去接受这些信息的话，给你多少时间也是不够
的。

一开始我在to do list里把微博和人人这种事情统一丢到晚上10点后，不过有时奏效，
有时不奏效。深入分析之，其中一个原因是上微博实在太容易了。打开Chrome，
常去网站里第一个就是它，点击一下，就到了你想到的地方。我也试过将它从常去网站里移
除，不过。。。你在浏览器里输入w，然后回车，就到了weibo.com/***，:(

于是，就有了Website Postponer。这个插件的设计哲学(说大了哈:P)，就是使你上某些
网站变得麻烦。为什么这样说呢？这个插件说白了就是我让一个程序来监督我执行那条to 
do list里把微博和人人放到晚上10点后才上的task。我可以设置一些网站，然后设置一个
时间，那么在这个时间之前，你打开这些网站都会被重定向到一个特定的页面。
这个页面是我自己设计的一个页面，上面写些话，比如：亲，不要老是成天想着刷微博云云。
人丑就要多读书之类的:P。再配些图。anyway，这种方式就像你想刷微博的时候，
有个人跑出来提醒你，不要刷微博，好好学习，天天向上，哈哈。当然了，工具永远只能是
工具，如果你非常非常想刷微博，那直接在设置页面把网站清除，或是干脆直接卸了这个插
件就OK了。所以，它起到的只是一个限制加提醒的作用，协助那些想限制自己上某些网站从
而减少时间浪费的人，如果在这些限制和提醒下，你还是想上那个网站，那没人管得住你。

废话说完，进入正题。

## 设计要素

主要就两项：

* 网站添加与清除
* 时间设置

添加完网站，设置完时间，你就无法在这个时间之前访问这些上了黑名单的网站了。
无图无真相，上图才是王道。

<img src="/assets/img/2012/12/14/option.png" />

这个图看起来果然很傻很天真，纯table搞出来的。以后如果有需要再做得漂亮一些，
现在这样就OK了。

这是插件里唯一需要与用户交互的页面，其他的都工作在后台了。

## 代码实现

这应该是我面向主题的第二个Chrome插件开发了，为什么叫面向主题？因为第一个插件
Url Striper写了好几个不同的版本，它是用来解决Google搜索页面中点击链接后的重定向
问题。因为重定向后的网址经常打不开，所以写了这么一个插件来解决。

Chrome插件的开发并不难，360公司的工程师们把Chrome插件的开发文档翻译成了中文，
这对想学Chrome插件开发的人来说是不小的福音。请戳：
[我是开发文档](http://open.chrome.360.cn/extension_dev/overview.html)

OK，开始吧。

首先，每个扩展，可安装的WebApp，皮肤都会有一个JSON格式的manifest文件，叫
manifest.json， 提供一些重要的信息。本插件的manifest.json如下，
注释已经写在每一行后面，需要详细讲解的会在下文给出。

	{
	  "name": "Website Postponer", //插件的名字，该名字会显示在扩展程序页面
	  "version": "12.12.13", //插件版本，你自己定。
	  "description": "Postpone the time you go to the specified websites",//插件描述
	  "background": { "scripts": ["background.js"] },//背景页脚本
	  "page_action" ://定义处理特定页面的事件
	  {
		"default_icon" : "images/icon-19.png",//出现在地址栏的图标
		"default_title" : "Settings"//鼠标停留在图标上显示的文字
	  },
	  "permissions" : [//扩展或app使用的权限，tabs声明使用标签页的权限
		"tabs"
	  ],
	  "icons" : {//图标，一般有三种大小：16/48/128，在不同的地方使用
		"48" : "images/icon-48.png",
		"128" : "images/icon-128.png"
	  },
	  "options_page": "options.html",//选项页，就是上一节那张图
	  "manifest_version": 2 //manifest文件的版本号
	}

上面有几个地方需要着重讲一下。

background.js：
扩展常常需要用一个单独的长时间运行的脚本来管理一些任务或者状态，这个脚本即为背景
页。它可以是一个html，然后在html中引入一些js脚本。比如：bg.html,然后里面引入
各种js脚本，而这些js脚本单独放在另外一个文件夹。当然，如果需要使用的函数不多，
那直接用一个背景的js脚本就OK了，不需要任何的html，就像上面的代码一样，只有一个
background.js。这个background.js在扩展的整个生命周期都存在。

Page Actions：
page action是什么呢，一图胜千言。从开发文档中盗张图来用：

<img src="/assets/img/2012/12/14/pageaction.png" />

那个地址栏中的RSS应用(黄色的那个图标)就使用了page action。

说到page action，就要顺便提一下browser action，browser action是什么呢？
还是看图：

<img src="/assets/img/2012/12/14/browseraction.png" />

那个gmail应用就使用了browser action。

当时在写代码时，本来顺理成章地想用browser action的，因为不管在哪个页面图标都
会显示出来，设置就很方便。而page action可以设置在哪些页面出现，哪些页面不出现。
后来我想，我本来就是想使登录某些网站变得麻烦的，那应该让设置也变得麻烦才对。
所以最后选了page action，并设置它只在扩展页中出现。即，你只有进入了Chrome的设置
中的扩展程序项，它才会显示出来。看图：

<img src="/assets/img/2012/12/14/ext.png" />

只有在上述页面中，才会显示红色圈里的图标。点击它，进入选项页进行设置。选项页后面
会讲。

options_page：
指定选项页为options.html。选项页是为了让用户可以设定扩展的功能或是参数。
因为该插件需要用户设置网址和时间，所以需要这么一个选项页。
选项页的图已经在第二节看过了，代码如下：

	<html>
	<head><title>Options</title>
	<script type="text/javascript" src="options.js"></script>
	</head>

	<body>
	<center>

	<table height="400" width="600" border="1">
	  <tr height="60">
	    <td align="top">
			<p>选择时间:</p>
		</td>
		<td>
			<select id="time">
				<option value="2">02:00</option>
				<option value="4">04:00</option>
				<option value="6">06:00</option>
				<option value="8">08:00</option>
				<option value="10">10:00</option>
				<option value="12">12:00</option>
				<option value="14">14:00</option>
				<option value="16">16:00</option>
				<option value="18">18:00</option>
				<option value="20">20:00</option>
				<option value="22">22:00</option>
				<option value="24">24:00</option>			
			</select>
		</td>
	    <td>
			<p>在以下时刻到来前，无法访问列表中的网址:</p>
			<label id="selectedtime">  </label>
		</td>
	  </tr>
	  <tr>
	    <td valign="middle" width="40%" height="60" colspan="2">
			<form>
				<p>http://<input type="text" id="url" /></p>
			</form>
		</td>
	    <td>
			<p>延迟访问名单:</p> 
		</td>
	  </tr>
	  <tr>
	    <td align="right" valign="bottom" colspan="2">
			<button id="btnSave">保存</button>
			<button id="btnClear">清除</button>
		</td>
		<td valign="top">
			<label id="weblist"></label>
		</td>
	  </tr>
	</table>

	</center>


	</body>
	</html>

这里有两个需要注意的地方：

1. button都没有定义onClick事件
1. html中没有内嵌的js脚本，而是通过src="options.js"引入

说到这个，又和manifest的最后一个参数：manifest_version相关。
manifest\_version是manifest自身文件的版本号，用整数表示。从2012年底左右，
开发者必须使用版本号2，也就是版本号1已经不可用了。你如果这里写版本号为1，打包或
是直接载入时，chrome就会向你报错。使用版本号2之后，由于安全性的需要，选项页
也就是上文的options.html中如果内嵌了javascript脚本，将不被执行。所以，
所有的js脚本都要移到外面的一个js文件，然后由options.html来引用。此外，
不能直接在元素上定义事件，比如你在button上定义onClick事件，并且也已经正确引用了
包含对应函数的js文件，它一样是不会执行的。那如果这样的话，我们该怎么来响应这些事
件呢？使用事件监听。看如下代码：

	document.addEventListener('DOMContentLoaded', function () {
	  document.querySelector('#btnSave').addEventListener('click', save_options);  //通过id找到相应元素
	  document.querySelector('#btnClear').addEventListener('click', clear_options);
	  window.addEventListener('load', restore_options);
	});
	
document.querySelector()找到相应id的元素，然后设置事件监听器。
第一二行代码监听两个按钮的点击事件，第三行代码监听页面的载入事件。

看起来是安全不少，不过一开始不知道，折腾了好久。最后还是要感谢StackOverflow
上的同学们，好多问题都能在上面找到答案。以下链接是Chrome Extensions文档里
关于此的解释：

<https://developer.chrome.com/extensions/contentSecurityPolicy.html#H3-1>

选项页options.html中事件对应的执行代码在options.js中，options.js文件如下：
	

	function checkExisted(newUrl){ //检查加入的网址是不是已经在列表中了
		var num = localStorage["count"];
		for(var i=0; i<num; ++i){
			var urli = "url" + i;
			var url = localStorage[urli];
			if(newUrl==url) return true;
		}
		return false;
	}
	// Saves options to localStorage.
	function save_options() {//用户填完信息后，保存选项，保存到localStorage
	  var select = document.getElementById("time");  //get the time
	  var time = select.children[select.selectedIndex].value;
	  var timeshow = select.children[select.selectedIndex].text;
	  localStorage["time"] = time;
	  localStorage["timeshow"] = timeshow;
	  //alert(time);

	  var url = document.getElementById("url").value;  //get the url
	  if(url){	//url: not null
	    var existed = checkExisted(url);
		if(!existed){	// add url when it is not existed in the localStorage
			var num = localStorage["count"];
			var urli = "url" + num;
			localStorage[urli] = url;
			localStorage["count"] = parseInt(num) + 1;
		}
	
	  }
	  
	  //alert(url);
	  
	  restore_options();
	}
	// Restores value from localStorage.
	function restore_options() {//当打开选项页时，恢复保存的信息
	  var timeshow = localStorage["timeshow"];
	  document.getElementById("selectedtime").innerHTML = ""; //clear the old time first
	  if (timeshow) {
	    document.getElementById("selectedtime").innerHTML = timeshow;
	  }
	  var num = localStorage["count"];
	  document.getElementById("weblist").innerHTML = ""; //clear the old weblist first
	  for(var i=0; i<num; ++i){
	    var urli = "url" + i;
		var url = localStorage[urli];
		if(url) {
			document.getElementById("weblist").innerHTML = document.getElementById("weblist").innerHTML + "</br>" + url;
		}
	  }
	  
	  
	}

	function clear_options() {//清除选项
	  var num = localStorage["count"];
	  for(var i=0; i<num; ++i){
	    var urli = "url" + i;
		localStorage[urli] = "";
		//if(!localStorage[urli]) alert("done");
	  }
	  localStorage["count"] = 0;
	  localStorage["time"] = "";
	  localStorage["timeshow"] = "";
	  restore_options();
	}
	//添加事件监听器
	document.addEventListener('DOMContentLoaded', function () {
	  document.querySelector('#btnSave').addEventListener('click', save_options);  //通过id找到相应元素
	  document.querySelector('#btnClear').addEventListener('click', clear_options);
	  window.addEventListener('load', restore_options);
	});

当用户填写完信息后，点击保存。save_options函数会从网页中找到相应的元素，
取出其相应的值，并将它保存在localStorage中。localStorage是html5中的新特性，
只要你的浏览器不是非常老掉牙，一般而言都是可以支持的。
localStorage以key/value的方式存储在浏览器中，没有时间限制。在存储的时候，
程序会对输入的URL会判断是否为空及是否已经存储过这个URL，当此URL为新的URL时，
存储它。并使URL计数器加1。

用户也可以点击按钮清除数据。restore_options函数在每次加载页面时将之前存储的值
加载到相应的区域显示，同时当设置有变化时它也负责刷新页面数据的显示。

最后是背景页的js脚本，如下：

	//localStorage只初始化一次
	if(!localStorage["count"])	//init only once
		localStorage["count"] = 0;  //the total number of urls

	// Called when the url of a tab changes.
	function showPageAction(url, tabId) {//设置在哪些页面显示图标
	  // If the url is:chrome://chrome/extensions
	  if (url.indexOf('chrome://chrome/extensions') > -1) {
	    // ... show the page action.
	    chrome.pageAction.show(tabId);
	  }
	};
	//如果网站在列表中，且时间未到，重定向。目前我是将它重定向到google.ca
	function blockWebsite(newUrl, tabId, tab) {
		if(!newUrl) return;
		var num = localStorage["count"];
		var time = localStorage["time"];
		var today=new Date();
		var now=today.getHours();
		for(var i=0; i<num; ++i){
			var urli = "url" + i;
			var url = localStorage[urli];
			if(newUrl.indexOf(url)>-1 && now<time) { //if the website is in the list and the time is less what you set,redirect.
				chrome.tabs.update(tabId, { url: "http://www.google.ca" });	
				break;
			}
		}
	}
	// Listen for any changes to the URL of any tab.
	chrome.tabs.onUpdated.addListener(function(tabId, changeInfo, tab){
		var myurl = tab.url;
		showPageAction(myurl, tabId); //show pageAction or not
		blockWebsite(myurl, tabId); //if the website is in the list, block it;
	});

	//open the options page
	function openOptions(){
		var url = "options.html";
		var fullUrl = chrome.extension.getURL(url);//chrome-extension://your extension id//options.html	
		chrome.tabs.getAllInWindow(null, function(tabs){
			for(var i in tabs){
				var tab = tabs[i];
				if(tab.url == fullUrl){//如果选项页已经打开了，就定位到那
					chrome.tabs.update(tab.id, {selected: true});
					return;
				}
			}
			//如果选项页没有打开，则打开它
			chrome.tabs.getSelected(null, function(tab){
				chrome.tabs.create({ url: url, index: tab.index+1});
			});
		});
	}
	//点击图标时，调用openOptions函数
	chrome.pageAction.onClicked.addListener(openOptions);
	
背景页会监听两个事件。

1. 当page action图标疲点击时，调用openOptions来打开选项页。如果选项页已经打开，
则直接定位到对应的标签即可；如果没有，则新打开一个标签页来显示它。
1. 当标签更新时，调用showPageAction来判断是否需要显示page action的图标；同时调用
blockWebsite来判断当前网页是否在你的"黑名单"中，如果是而且时间还没到你设置的时间，
那么，网页会被重定向到google.ca。(由于自己想要的页面还没做，所以先定向到google，
这是我每天用的最多的网站了)

OK，这就是我写的一个简单的Chrome插件。有时间再去改进它。

如果有兴趣安装玩一下，请戳下面的链接：

[WebsitePostponer](/assets/dl/WebsitePostponer.crx)

源代码在我的GitHub上：

[WebsitePostponer源码](https://github.com/Hawstein/WebsitePostponer)

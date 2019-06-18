---
layout: post
title: 百度短网址应用
date: 2015-02-09 11:42
tags: webapp, javascript
cover: http://greatghoul.b0.upaiyun.com/1502/FvUGgWkdKLc9.png
---

![](http://greatghoul.b0.upaiyun.com/1502/FvUGgWkdKLc9.png)

短网址应用现在已经非常普遍，我以前一起用的 [Google URL Shortener][1]，
它还有一个非常方便的[浏览器扩展][2]，但最近因为墙的增高，这个服务在国内用
起来，就不那么方便了。

当然，国内也有一些短网址的应用：

- 腾讯 <http://url.cn/>
- 新浪 <http://t.cn/>
- 百度 <http://dwz.cn/>

前两个没有对外开放接口，只有百度的还比较有良心一些，其实新浪的是有一个[接口][3]的，
不过调用时还必须注册个什么 Key，非常麻烦，所以使用百度的短网址，还是相对靠谱一些的。

但百度的短网址没有自己的浏览器扩展，连 Bookmarklet 都找不到，虽然 Chrome Webstore
上面有一些非官方的，但是不少都加了料，实在不敢使用。

## 怎么办？

![](http://greatghoul.b0.upaiyun.com/1502/vIEnqf-9sKAr.png)

程序员不可能这么轻易被打败，我来自己做一个。

仔细想一想，其实很简单嘛：**传入一个 url，调用百度的 API 将其缩短，然后返回给用户。**

就是它了，甚至都不需要服务端。

## 百度短网址

项目：<https://github.com/greatghoul/dwz>

演示：<http://dwz.ghoulmind.com/>

![](http://greatghoul.b0.upaiyun.com/1502/W3dStVl_7CqY.png)


> 这里还提供了一个 Bookmarklet 用于快速缩短网页的地址
> 
> <a href="javascript:(function(){window.open('http://dwz.coding.io?url='+encodeURIComponent(location.href),'_blank','width=450,height=260');})()" class="btn btn-success">缩短网址</a>
> <span class="text-info">&lt;- 将我拖动到书签栏</span>

## 跨域问题

**不是程序员可以绕过了。**

百度的 [API][4] 是不能跨域调用的 POST 请求，纯静态的页面是没有办法处理的，
但倔强的程序员怎么可能甘心，所以有了这个项目。

<https://coding.net/u/greatghoul/p/httpget/git>

它的作用是把非 GET 的请求通过一个中间人，改变成 GET 的方式以供 JS 跨域调用。

> http://hget.sinaapp.com/**post/http://dwz.cn/create.php**

这样就可能解决跨域问题了

    $.getJSON('http://hget.sinaapp.com/post/http://dwz.cn/create.php', data, callback);

这个服务的实现还是有些问题的，但暂时勉强能够工作，如果有感兴趣的朋友，可以一起关注一下。

[1]: https://goo.gl/
[2]: https://chrome.google.com/webstore/detail/googl-url-shortener/iblijlcdoidgdpfknkckljiocdbnlagk
[3]: http://open.weibo.com/wiki/Short_url/shorten
[4]: http://help.baidu.com/question?prod_en=webmaster&class=%CD%F8%D2%B3%CB%D1%CB%F7%CC%D8%C9%AB%B9%A6%C4%DC&id=1000913#05
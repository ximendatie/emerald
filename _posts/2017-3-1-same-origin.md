---
title: 跨域问题总结 —— 同源政策
---

跨域问题对于前端开发人员来说并不陌生，当你在控制台看到`No "Access-Control-Allow-Origin"header is present on the request resource` 那就代表着你可能碰到跨域问题了，它在ajax、cookie等情景中出现的概率还是很高的，我们这里把跨域问题出现的原因，以及跨域解决方法进行分类总结。

<!--more-->

## 什么是同源

同源政策1995年被引入浏览器后被广泛使用，目前所有浏览器都实行这个政策。   
同源政策最初是为了限制不同网页间Cookie的访问，众所周知Cookie很多情况下会被用来登陆信息存储，如果A网页信息被B网页随便访问，那么将带来很大的安全问题。所以就把Cookie的访问限制在同源范围内。  

### 同源条件
所谓同源，具备三个基本条件：

    1. 协议相同
    2. 域名相同
    3. 端口相同

这是个很严格的条件，基本能够保障网页不会被别的网页随便窃取信息，我们通过一些例子来看哪些是同源哪些不是。

我们以http://www.example.com/dir/page.html 为例，来看下表的比对结果：

|Compared URL	|Outcome	|Reason|
|:-------------|:------|:-----|
|http://www.example.com/dir/page2.html	|是|	符合条件
|http://www.example.com/dir2/other.html	|是|	符合条件
http://username:password@www.example.com/dir2/other.html	|是|	符合条件
http://www.example.com:81/dir/other.html	|否|	端口不同
https://www.example.com/dir/other.html	|否	|协议不同
http://en.example.com/dir/other.html	|否|	域名不同
http://example.com/dir/other.html	|否|	域名不同
http://v2.www.example.com/dir/other.html	|否	|域名不同

### 同源的限制范围 

同源政策的出现自然是为了网页安全，前面我们说过同源政策起初是为了限制网页间的Cookie窃取，随着互联网发展同源政策也越来越严格，也就意味着碰到跨域问题的情景就会更多。

以下三种行为都会被同源政策限制：

    1. Cookie、LocalStorage 和 IndexDB 无法读取
    2. DOM 无法获得
    3. AJAX 请求不能发送

同源政策虽然会在安全性上给网站带来保障，但是同时也会成为很多信息传递情景的一大障碍，下篇文章我们将主要探讨如何解决跨域问题。

### 参考

>[Same-origin policy](https://en.wikipedia.org/wiki/Same-origin_policy) Wikipedia  
>[Web开发中跨域的几种解决方案](http://harttle.com/2015/10/10/cross-origin.html)  Harttle  
>[浏览器同源政策及其规避方法](http://www.ruanyifeng.com/blog/2016/04/same-origin-policy.html) 阮一峰  
>[跨域资源共享 CORS 详解](http://www.ruanyifeng.com/blog/2016/04/cors.html) 阮一峰  
>[详解js跨域问题](https://segmentfault.com/a/1190000000718840) trigkit4



---
title: 跨域问题总结 —— 同源政策
---

跨域问题总结 —— 解决方法

上篇文章介绍了跨域问题的由来和特点，这篇文章我们来谈谈怎么有效解决跨域问题，并将解决办法分为了三个问题场景和五个具体类型，方便大家理解和记忆。

<!--more-->

## 回顾同源政策的限制

上一篇文章我们讲到同源政策的限制范围，主要是下面三种：

    1. Cookie、LocalStorage 和 IndexDB 无法读取。
    2. DOM 无法获得。
    3. AJAX 请求不能发送。

既然同源政策会限制这些行为，那么不可避免的我们可能会因为实现这些行为而碰到跨域问题，下面以此三种情景进行介绍跨域问题的解决方案。

## 一、 Cookie场景



### 参考

>[Same-origin policy](https://en.wikipedia.org/wiki/Same-origin_policy) Wikipedia  
>[Web开发中跨域的几种解决方案](http://harttle.com/2015/10/10/cross-origin.html)  Harttle  
>[浏览器同源政策及其规避方法](http://www.ruanyifeng.com/blog/2016/04/same-origin-policy.html) 阮一峰  
>[跨域资源共享 CORS 详解](http://www.ruanyifeng.com/blog/2016/04/cors.html) 阮一峰  
>[详解js跨域问题](https://segmentfault.com/a/1190000000718840) trigkit4



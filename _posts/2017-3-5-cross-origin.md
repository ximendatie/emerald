---
title: 跨域问题总结 —— 解决方法
---



上篇文章介绍了跨域问题的由来和特点，这篇文章我们来谈谈怎么有效解决跨域问题，并将解决办法分为了三个问题场景和7个具体解决方案，方便大家理解和记忆。

<!--more-->

## 回顾同源政策的限制

上一篇文章我们讲到同源政策的限制范围，主要是下面三种：

    1. Cookie、LocalStorage 和 IndexDB 无法读取。
    2. DOM 无法获得。
    3. AJAX 请求不能发送。

既然同源政策会限制这些行为，那么不可避免的我们可能会因为实现这些行为而碰到跨域问题，下面以此三种情景进行介绍跨域问题的解决方案。

## 一、 Cookie场景

对于需要实现注册登陆的的网页，Cookie想必开发者一点也不陌生，Cookie 是服务器写入浏览器的一小段信息，只有同源的网页才能共享，这是网页安全的一大关键。  
不过两个一级域名相同而二级域名不同的页面可能是需要共享Cookie的，比如 http://news.qq.com 与 http://qq.com 之间，肯定是一个qq账号登陆，避免重复登陆的繁琐过程。  

### 解决办法1 ——   document.domain

举例来说，A网页是http://w1.example.com/a.html，B网页是http://w2.example.com/b.html  
那么只要设置相同的document.domain，两个网页就可以共享Cookie。`document.domain = 'example.com';`

现在，A网页通过脚本设置一个 Cookie。`document.cookie = "test1=hello";`

B网页就可以读到这个 Cookie。`var allCookie = document.cookie;`
    
**注意:**  
    
    这种方法只适用于 Cookie 和 iframe 窗口，LocalStorage 和 IndexDB 无法通过这种方法，规避同源政策。

另外，**服务器**也可以在设置Cookie的时候，指定Cookie的所属域名为一级域名，比如.example.com。  
`Set-Cookie: key=value; domain=.example.com; path=/`  
这样的话，二级域名和三级域名不用做任何设置，都可以读取这个Cookie。

## 二、 iframe 场景

如果两个网页不同源，就无法拿到对方的DOM。典型的例子是iframe窗口和window.open方法打开的窗口，它们与父窗口无法通信。
比如，父窗口运行下面的命令，如果iframe窗口不是同源，就会报错。

```javascript
document.getElementById("myIFrame").contentWindow.document
// Uncaught DOMException: Blocked a frame from accessing a cross-origin frame.
```

上面命令中，父窗口想获取子窗口的DOM，因为跨源导致报错。
反之亦然，子窗口获取主窗口的DOM也会报错。

```javascript
window.parent.document.body
// 报错
```

如果两个窗口一级域名相同，只是二级域名不同，那么设置上一节介绍的document.domain属性，就可以规避同源政策，拿到DOM。

对于完全不同源的网站，目前有三种方法，可以解决跨域窗口的通信问题。

> 解决办法2. 片段识别符（fragment identifier）
> 解决办法3. window.name
> 解决办法4. 跨文档通信API（Cross-document messaging）

### 解决办法2 —— 片段识别符（fragment identifier）

片段标识符（fragment identifier）指的是，URL的#号后面的部分，比如http://example.com/x.html#fragment的#fragment。如果只是改变片段标识符，页面不会重新刷新。
父窗口可以把信息，写入子窗口的片段标识符。

``` javascript
var src = originURL + '#' + data;
document.getElementById('myIFrame').src = src;
```
子窗口通过监听hashchange事件得到通知。

``` javascript
window.onhashchange = checkMessage;

function checkMessage() {
  var message = window.location.hash;
  // ...
}
```

同样的，子窗口也可以改变父窗口的片段标识符。

`parent.location.href= target + "#" + hash;`

### 解决办法3 —— window.name
浏览器窗口有window.name属性。这个属性的最大特点是，无论是否同源，只要在同一个窗口里，前一个网页设置了这个属性，后一个网页可以读取它。
父窗口先打开一个子窗口，载入一个不同源的网页，该网页将信息写入window.name属性。

`window.name = data;`
接着，子窗口跳回一个与主窗口同域的网址。

`location = 'http://parent.url.com/xxx.html';`
然后，主窗口就可以读取子窗口的window.name了。

`var data = document.getElementById('myFrame').contentWindow.name;`
这种方法的优点是，window.name容量很大，可以放置非常长的字符串；缺点是必须监听子窗口window.name属性的变化，影响网页性能。

### 解决方法4 —— window.postMessage
上面两种方法都属于破解，HTML5为了解决这个问题，引入了一个全新的API：跨文档通信 API（Cross-document messaging）。
这个API为window对象新增了一个window.postMessage方法，允许跨窗口通信，不论这两个窗口是否同源。
举例来说，父窗口`http://aaa.com向子窗口http://bbb.com`发消息，调用postMessage方法就可以了。

``` javascript
var popup = window.open('http://bbb.com', 'title');
popup.postMessage('Hello World!', 'http://bbb.com');
```
postMessage方法的第一个参数是具体的信息内容，第二个参数是接收消息的窗口的源（origin），即"协议 + 域名 + 端口"。也可以设为*，表示不限制域名，向所有窗口发送。
子窗口向父窗口发送消息的写法类似。
`window.opener.postMessage('Nice to see you', 'http://aaa.com');`
父窗口和子窗口都可以通过message事件，监听对方的消息。

``` javascript
window.addEventListener('message', function(e) {
  console.log(e.data);
},false);
```
message事件的事件对象event，提供以下三个属性。
event.source：发送消息的窗口
event.origin: 消息发向的网址
event.data: 消息内容
下面的例子是，子窗口通过event.source属性引用父窗口，然后发送消息。

``` javascript
window.addEventListener('message', receiveMessage);
function receiveMessage(event) {
  event.source.postMessage('Nice to see you!', '*');
}
```
event.origin属性可以过滤不是发给本窗口的消息。

``` javascript
window.addEventListener('message', receiveMessage);
function receiveMessage(event) {
  if (event.origin !== 'http://aaa.com') return;
  if (event.data === 'Hello World') {
      event.source.postMessage('Hello', event.origin);
  } else {
    console.log(event.data);
  }
}
```


上面代码中，子窗口将父窗口发来的消息，写入自己的LocalStorage。
父窗口发送消息的代码如下。

``` javascript
var win = document.getElementsByTagName('iframe')[0].contentWindow;
var obj = { name: 'Jack' };
win.postMessage(JSON.stringify({key: 'storage', data: obj}), 'http://bbb.com');
```
加强版的子窗口接收消息的代码如下。

``` javascript
window.onmessage = function(e) {
  if (e.origin !== 'http://bbb.com') return;
  var payload = JSON.parse(e.data);
  switch (payload.method) {
    case 'set':
      localStorage.setItem(payload.key, JSON.stringify(payload.data));
      break;
    case 'get':
      var parent = window.parent;
      var data = localStorage.getItem(payload.key);
      parent.postMessage(data, 'http://aaa.com');
      break;
    case 'remove':
      localStorage.removeItem(payload.key);
      break;
  }
};
```

加强版的父窗口发送消息代码如下。

``` javascript
var win = document.getElementsByTagName('iframe')[0].contentWindow;
var obj = { name: 'Jack' };
// 存入对象
win.postMessage(JSON.stringify({key: 'storage', method: 'set', data: obj}), 'http://bbb.com');
// 读取对象
win.postMessage(JSON.stringify({key: 'storage', method: "get"}), "*");
window.onmessage = function(e) {
  if (e.origin != 'http://aaa.com') return;
  // "Jack"
  console.log(JSON.parse(e.data).name);
};
```

## 三、 Ajax场景
上面两种场景我们讲述窗口间的跨域，使用了四种方法，这里我们对于服务器通信也就是Ajax所面临的跨域问题进行总结。
同源政策规定，AJAX请求只能发给同源的网址，否则就报错。
除了架设服务器代理（浏览器请求同源服务器，再由后者请求外部服务），有三种方法规避这个限制。

    5.JSONP
    6.WebSocket
    7.CORS

### 解决方法5 —— JSONP
JSONP是服务器与客户端跨源通信的常用方法。**最大特点就是简单适用，老式浏览器全部支持**，服务器改造非常小。  
它的基本思想是，网页通过添加一个`<script>`元素，利用src向服务器请求资源，这种做法不受同源政策限制；服务器收到请求后，会返回资源，我们借此机会将数据放在一个回调函数将此函数当作资源传回来，写入到`<script>`标签内。  

首先，网页动态插入`<script>`元素，由它向跨源网址发出请求。

```javascript
function addScriptTag(src) {
  var script = document.createElement('script');
  script.setAttribute("type","text/javascript");
  script.src = src;
  document.body.appendChild(script);
}

window.onload = function () {
  addScriptTag('http://example.com/ip?callback=foo');
}

function foo(data) {
  console.log('Your public IP address is: ' + data.ip);
};
```
上面代码通过动态添加`<script>`元素，向服务器example.com发出请求。注意，该请求的查询字符串有一个callback参数，用来指定回调函数的名字，这对于JSONP是必需的。
服务器收到这个请求以后，会将数据放在回调函数的参数位置返回。
```javascript
foo({
  "ip": "8.8.8.8"
});
```
由于`<script>`元素请求的脚本，直接作为代码运行。这时，只要浏览器定义了foo函数，该函数就会立即调用。作为参数的JSON数据被视为JavaScript对象，而不是字符串，因此避免了使用JSON.parse的步骤。

### 解决方法6 —— WebSocket
WebSocket是一种通信协议，使用ws://（非加密）和wss://（加密）作为协议前缀。该协议不实行同源政策，只要服务器支持，就可以通过它进行跨源通信。
下面是一个例子，浏览器发出的WebSocket请求的头信息（摘自维基百科）。

    GET /chat HTTP/1.1
    Host: server.example.com
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: x3JJHMbDL1EzLkh9GBhXDw==
    Sec-WebSocket-Protocol: chat, superchat
    Sec-WebSocket-Version: 13
    Origin: http://example.com

上面代码中，有一个字段是Origin，表示该请求的请求源（origin），即发自哪个域名。
正是因为有了Origin这个字段，所以WebSocket才没有实行同源政策。因为服务器可以根据这个字段，判断是否许可本次通信。如果该域名在白名单内，服务器就会做出如下回应。

    HTTP/1.1 101 Switching Protocols
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Accept: HSmrc0sMlYUkAGmm5OPpG2HaGWk=
    Sec-WebSocket-Protocol: chat


4.3 CORS
CORS是跨源资源分享（Cross-Origin Resource Sharing）的缩写。它是W3C标准，是跨源AJAX请求的根本解决方法。相比JSONP只能发GET请求，CORS允许任何类型的请求。 
CORS需要浏览器和服务器同时支持。目前，所有浏览器都支持该功能，IE浏览器不能低于IE10。
整个CORS通信过程，都是浏览器自动完成，不需要用户参与。对于开发者来说，CORS通信与同源的AJAX通信没有差别，代码完全一样。浏览器一旦发现AJAX请求跨源，就会自动添加一些附加的头信息，有时还会多出一次附加的请求，但用户不会有感觉。
因此，实现CORS通信的关键是服务器。只要服务器实现了CORS接口，就可以跨源通信。
具体介绍可以参考 阮一峰的[跨域资源共享 CORS 详解](http://www.ruanyifeng.com/blog/2016/04/cors.html)

### 参考

>[Same-origin policy](https://en.wikipedia.org/wiki/Same-origin_policy) Wikipedia  
>[Web开发中跨域的几种解决方案](http://harttle.com/2015/10/10/cross-origin.html)  Harttle  
>[浏览器同源政策及其规避方法](http://www.ruanyifeng.com/blog/2016/04/same-origin-policy.html) 阮一峰  
>[跨域资源共享 CORS 详解](http://www.ruanyifeng.com/blog/2016/04/cors.html) 阮一峰  
>[详解js跨域问题](https://segmentfault.com/a/1190000000718840) trigkit4



---
title: JavaScript 单引号与双引号
---



经验使用中碰到的js单引号双引号使用差别，做一下总结。两者一般使用上区别不大，和个人习惯有关，不过还是会有一些注意点。

<!--more-->

1. json格式中一定key和value一定用双引号，因为很多语言只能解析双引号的格式，为了接口通用性，必须使用双引号来包围key和value

    ```javascript
    JSON.parse('{"a":1,"b":2}'); //正确
    JSON.parse("{'a':1,'b':2}");//错
    ```

2. 引号嵌套，使用不同类型的引号，如果需要显示引号字符，使用反斜杠\(html页面不能使用\进行转义)，进行转义。
    
    ```javascript
    <input type="button" onclick="alert("1")">//错
    <input type="button" onclick="alert('1')">//对
    <input type="button" onclick="alert('\"1\"')">//错
    console.log("abc\"def\"ghi");//对
    ```


待续...



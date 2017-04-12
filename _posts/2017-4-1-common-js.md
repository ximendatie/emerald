---
title: JavaScript常用代码块记录
---
这里记录个人常用到的一些代码块，在写过程文件的时候能够提高一些效率，也方便以后温习。

<!--more-->

## 目录

[自己实现时间片段](#sleep)


### <span id='sleep'>时间片段</span>

时间片段一般会使用setTimeout和setInterval，不过一些特殊情况可能还是需要自己写。

```javascript
function sleep(numberMillis) {  
    var now = new Date();  
    var exitTime = now.getTime() + numberMillis;  
    while (true) {  
        now = new Date();  
        if (now.getTime() > exitTime)  
            return;  
    }  
}  
```


This is a CSS example: 

{% highlight css linenos %}

body { background-color: #fff; }

h1 { color: #ffaa33; font-size: 1.5em; }

{% endhighlight %}

And this is a HTML example, with a linenumber: {% highlight html linenos %}

Example
{% endhighlight %}

Last, a Ruby example: {% highlight ruby linenos %}

def hello puts "Hello World!" end

{% endhighlight %}

{% highlight javascript linenos %}

function sleep(numberMillis) {  
    var now = new Date();  
    var exitTime = now.getTime() + numberMillis;  
    while (true) {  
        now = new Date();  
        if (now.getTime() > exitTime)  
            return;  
    }  
} 

{% endhighlight %}



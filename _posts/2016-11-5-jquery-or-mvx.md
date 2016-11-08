---
title: 是jQuery还是MV*，这是个问题
---

在那个ie6、ie7横行的年代，在那个web前端行业刚刚萌芽的时代，jq曾是不可代替甚至标配的利器。时至今日，jq依旧受到大批开发者的钟情，然而，在mv*盛行的今天，jq之道似乎已经渐行渐远……

<!--more-->

有点标题党，jq（为了方便，以下用jq代替jQuery）祖宗岂是我等小辈能评头论足的。当然，所谓的何去何从，只是做一个预测和想象，纯属主观表达。

## 一、从入门到放弃
那时候天真地以为jq可以摆平一切，从此无所畏惧。却不知world从来不曾为哪个framework/library停下脚步。

而在人手一个mv\*的时代，jq已经慢慢淡出人们的视线。**angular、react、vue、ember、knockout**，等等后起之秀慢慢崛起并成为主流。表现为以下几点（以**react**为例）：

1)jq时代所提倡的样式、结构、行为分离的编程方式，已经慢慢被更为先进的component（组件化）思想所取代:

```html
/*
通过外联css与js实现样式、结构、行为分离
*/
<head>
  <link href="index.css" />
</head>
<body>
  <div class="box" id="box">
    hello, world!
  </div>
<script src="jquery.min.js"></script>
<script src="index.js"></script>
</body>
```

vs

```js
/*
一个jsx即一个完整独立的组件，直接引入了相关样式，html与js混合编写，三者关系紧密
*/
// index.jsx
import React from 'react'
require('index.scss')
const Box = React.createClass({
  render () {
    return (
      <div className="box">
        hello, world!
      </div>
    )
  }
})

export default Box
```

2）不再是选择器的重度依赖者，更多时候我们可以把事件绑定以及样式直接写在行间，这样做的好处很明显，一个是我们不再需要为获取而书写长长的各种嵌套的选择器而导致各种隐患；一个是使得目标更加清晰，操作更加便捷：

```html
<head>
  <link href="index.css" />
</head>
<body>
  <div class="box" id="box">
    hello, world!
  </div>
<script src="jquery.min.js"></script>
<script src="index.js"></script>
</body>
```

```css
// index.css
.box {
  background-color: 'green';
  color: 'red';
}
```

```js
// index.js
$(document).ready(function() {
  var $box = $('#box')
  $box.click(function() {
    $(this)
    .html('hello, jq!')
    .css({
      'color': '#fff'
    })
  })
})
```

vs

```js
// index.jsx
import React from 'react'
const Box = React.createClass({
  getInitialState () {
    return {
      target: 'world',
      styles: {
        backgroundColor: 'green',
        color: 'red'
      }
    }
  },
  render () {
    var _t = this

    return (
      <div className="box" style={_t.state.styles} onClick={_t.changeTarget}>
        hello, {_t.state.name}!
      </div>
    )
  },
  changeTarget () {
    var styles = this.state.styles
    styles.color = '#fff'
    this.setState({
      target: 'jq',
      styles: styles
    })
  }
})

export default Box
```

3）mv*使前端编程进入了高级模式。jq的出现给我们带来了一种新的、良好的编程模式，可能大家并没有注意到，举个栗子：

```js
// 传统的编程方式
var box = document.getElementById('box')
var items = box.getElementByTagName('li')

for (var i = 0; i < items.length; i++) {
  items[i].style.color = 'red'
  items[i].onclick = function () {
    alert(this.innerHTML)
  }
}
```
vs

```js
// jq模式
$('#box li')
.css({color: 'red'})
.click(function () {
  alert($(this).html())
}).css()
```

我们且不谈jq节约了多少代码，如何地简洁，我们只谈这样的模式带来的好处：

- 传统方式总共要在全局暴露/占用box、items、i（颇为人诟病的一例）几个变量，而jq则完全没有，全部都被匿名了，很大程度地减少了全局污染；
- 自动遍历；
- 链式调用，如果需要，我们可以一直无限长地调用下去。这使得操作更连贯并且更好地理解，每个操作都紧密联系在一起，又随时可以拆分；
- jq同时为我们屏蔽了浏览器的差异性，也就是我们平常说的兼容问题，在开发者手中，只有统一的api，没有不同的浏览器，这也是jq最大的一个优点，这使得开发者可以专注于业务而不必为浏览器的兼容问题苦恼。

最后再稍微补充一点不太准确的，就是性能问题，jq的api性能可能要比自己写的高些，当然这一点视个人水平不同而定。  

然而，jq也并没有从整个web应用的层面去解决问题，它只是在细节上对js做一些补充和装点。随着web前端工程越来越复杂，我们需要一些更高级的模式来解决问题。

随着nodejs的出现，前端开始进入工程化时代。继而requirejs、seajs等等一类amd/cmd框架出现，都是尝试去解决构建复杂应用的问题。由于这些都与jq可以共存，没有冲突，我们不做细说。

再然后出现的angular、react等一系列mv*的框架，其实就是提出了一种可行的、更高级的代码组织方式。同时，由于es6的出现，react的jsx结合es6特性如鱼得水。

（以下内容都是以react为参照）

这时候，jq开始显得有点尴尬了。

组件化使得jq的选择器无用武之地，你已经不能在`$(document).ready`里安全地获取元素，因为你不知道这个`element`什么时候`(un)mount`；
我们可以随便install一个轻量级的、纯粹的ajax package来替代jq的ajax；
我们通过改变state来代替直接对dom操作；
jq插件无法愉快使用，jq的存在意义就大打折扣了；
……
jq之道渐行渐远。

## 二、插件们的未来
jq那丰富多彩的插件世界，是一笔巨大的财富，如果jq退出历史舞台，那么那些依赖于jq的插件们，或者说，那些不依赖于jq的但没有cmd/amd化的插件们，也必然会没落。

但会总有人去做这样的工作的，不是吗？

事实上，大部分插件都可以通过以下方式很轻松地转化为cmd/amd（代码来自网络）：

```js
;(function(){
  function MyModule() {
    // ...
  }
  var moduleName = MyModule;
  if (typeof module !== 'undefined' && typeof exports === 'object' && define.cmd) {
    module.exports = moduleName;
  } else if (typeof define === 'function' && define.amd) {
    define(function() { return moduleName; });
  } else {
    this.moduleName = moduleName;
  }
}).call(function() {
  return this || (typeof window !== 'undefined' ? window : global);
});
```

当然jq本身是支持amd/cmd的。

但是很显然，jq已经不再是必要的了，开发者们会慢慢地转向新的社区，去为诸如react、vue等框架注入新力量。

与其说插件，不如直接说组件。amd/cmd改造只是第一步，其效果也只是勉强能用而已，但是要从一个传统插件完完全全改造成reactive，还是相当麻烦的。

在react中，一个组件的最终形态，是一个简单的被扩展了的标签，接收来自父组件的参数（states/props/methods）作为输入，并且输出view：

```js
// index.jsx
import React from 'react'
import Box from './component/Box'
const Index = React.createClass({
  getInitialState () {
    return {
      target: 'world',
      styles: {
        backgroundColor: 'green',
        color: 'red'
      }
    }
  },
  render () {
    var _t = this

    return (
      <div className="index">
        hello, {_t.state.target}!
        <Box style={_t.state.styles} target={_t.state.target} clickHandle={_t.changeTarget} />
      </div>
    )
  },
  changeTarget () {
    var styles = this.state.styles
    styles.color = '#fff'
    this.setState({
      target: 'jq',
      styles: styles
    })
  }
})

export default Index


// box.jsx
import React from 'react'
const Box = React.createClass({
  render () {
    var _t = this

    return (
      <div className="box" style={_t.props.styles} onClick={_t.props.clickHandle}>
        hello, {_t.props.target}!
      </div>
    )
  }
})

export default Box
```

## 三、尘埃落定
不过这么些年来，jq也不是毫无变化，jQuery2.0及后续版本将不再支持IE6/7/8浏览器，这是个好事，至少说明jq也是与时俱进的。

今年（2016）的6月3日，jQuery3.0发布；7月7日，jQuery3.1发布，这是本文发布时最新的一个版本。

现在还在使用jq的，大概就以下几种情况：

- 需要兼容旧ie的项目；
- 目前还在维护的旧项目；
- 入行新手；
- 坚持样式、结构、行为分离的开发者；
……

在人手一个mv\*的时代，jq能做出什么样的转变？答案是否定的，jq的优势已经不再，mv*框架屏蔽了dom的操作，变得透明，我们只需要关心数据，修改数据。用过的人都有过体会，离开jq，依然可以愉快玩耍。

jq也不太可能造个轮子，把自己整成一个mv*，如果这么做，那jq就是放着擅长的事不去做，而选择背离初衷。library毕竟不是framework。即便给jq装个模块加载器，也不会改变什么。

一句话：一旦我们习惯上了数据绑定视图的编程方式，就再也不想自己去手动操作dom了。

---

[原文链接](http://www.jianshu.com/p/1e13baaef514)



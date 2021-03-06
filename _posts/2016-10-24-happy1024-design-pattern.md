---
title: 借Javascript谈设计模式
---

今天10月24，先祝诸位程序员节日快乐.~

┈╭━━━━━━━━━━━╮┈   
┈┃╭━━━╮┊╭━━━╮┃┈   
╭┫┃┈❤️┈┃┊┃┈❤️┈┃┣╮  
┃┃╰━━━╯┊╰━━━╯┃┃   
╰┫╭━╮╰━━━╯╭━╮┣╯   
┈┃┃┣┳┳┳┳┳┳┳┫┃┃┈   
┈┃┃╰┻┻┻┻┻┻┻╯┃┃┈   
┈╰━━━━━━━━━━━╯┈    

今天我们借JS谈谈设计模式。
<!--more-->

模式应该算是js比较高级的语法了吧。 如果平时只是用用jQuery，写写功能函数，尚未使用或者意识到设计模式能够带来的意义，可能是你功力还不够深。

## 模式之辩

经常听人说，js是一个OO的语法，一切皆是对象。之所以这样说，是因为在js中，对象其实就是一个散列表(key/value).

```js
var obj = {
    
    name: "I am an Object",
    
    ability:function(){
    
        console.log(this.name);
    
    }
    
}
obj.ability();
```
 

这应该算是一个基本的对象了，基本功能还是有的。 之所以提及这个东西，就是因为在js中有很多概念都和对象有关，所以在这里就先给大家一个impression吧.
切入主题

### 工厂模式

写过代码有一定时间的淫应该都会了解一下这个模式吧。 其实说白的就是封装，将一段重用性高的代码，封装起来，以便多次调用。

老板来个栗子~
好嘞~

```js
 function Person(name,gender,age){

     var obj = new Object();

     obj.name = name;

     obj.gender = gender;

     obj.age = age;

     return obj;

 }

 var jimmy1 = Person("jimmy1","male",18);

 var jimmy2 = Person("jimmy2","male",19);

 console.log(jimmy1.name);

 //jimmy1

 console.log(jimmy2.name);

 //jimmy2
```

可以看到上面演示的， 将一个person给封装起来，可以创建无数多个人。 我操，那我以后就可以用这个写阿喂。额~ 可以是可以，不过有点 waste . 而且不能很好的说明你创建的到底是个什么东西，我们使用typeof,以及instanceof 来检验一下.

```js
 console.log(typeof jimmy1);  //"object"

 console.log(jimmy1 instanceof Object);  //true

 console.log(jimmy1 instanceof Person);  //false
```

我们细细看一下， 上面例子中，每个人都创建了一个Obejct() 的实例， 然后添加属性，方法，最后再返回。 这样写简单明了，但是我们是有技术洁癖的人。 所以我们要提取公因式，将重复的代码再提出出来。 比较官方的说法叫做，构造函数模式~

### 构造函数模式

构造函数~ 是什么勒。 咱们不弄虚的。

```js
 function Person(name,gender,age){

     this.name = name;

     this.gender = gender;

     this.age = age;

 }
 var jimmy1 = new Person("jimmy1",'male',18);

 jimmy1.name;

 // jimmy1

 var jimmy2 = new Person("jimmy2",'male',18);

 jimmy2.name;

 // jimmy2
```

这就是一个超级典型的构造函数模式, 有 this，有参数，有new,还有实例。

构造函数的原理其实和工厂模式的原理一样，就是创建一个对象，然后添加对应的属性和方法。

构造函数不是函数吗？怎么和对象扯上关系了。 别忘了我们开篇提及的 一切皆是对象。 我们来理一理过程：

（1）创建一个新对象。 // new Person(); 的作用

（2）将构造函数的作用域赋给新对象 //就是将this指向这个对象, 相当于上例中的obj

（3）执行构造函数里面的内容 //给新对象添加方法和属性, this就是你创建的Obj

（4）返回新对象 //就是Obj了

哈哈~ 是不是觉得写代码，也可以写出艺术的感觉嘞~其实构造函数模式应该算是工厂模式的一个升级版。 在每个实例上，会存在一个constructor的属性，来表示该对象的类型.

```js
 console.log(jimmy1.constructor);  //Person

 console.log(jimmy1 instanceof Person);  //true
```

这样我总算可以知道我创建的是不是 一个人了。但实际的情况是，我创建的是一个静态的人，他的动作我该怎么完成。 加呗~

```js
 function Person(name,gender,age){

     this.name = name;

     this.gender = gender;

     this.age = age;

     this.speak = function(){

         console.log("my name is " +this.name);

     }

 }

 var jimmy1 = new Person("jimmy1",'male',18);

 jimmy1.speak();

 //my name is jimmy1
```

恩，这样啊~ 一门高级语言，永远不会让你知道你不知道。

想一想，和工厂模式是一个道理，每次创建一个实例，我都得实例一个对象，而且记住函数也是对象。

上面的例子中，我在创建Person的实例时， 由于内部还有一个speak的函数，我还得创建另外一个speak的实例对象。而且这些实例都是建在哪里勒，呵呵，你的内存中~

实际情况是，如果我创建了>1000 个人， 我比A bra 还小的4G内存怎么活啊。 js 用于不会让你失望的。

在社区的衍生中，出现了另外一个变体: 原型模式
(Ps:这是我在面试鹅厂的时候，一个经典问题)

### 原型模式

提及原型模式，就必须说明一下原型这个概念。想一想我们创建对象的时候做了什么~

 ```js
 var obj = new Object();
 ```

其实上面的写法，是创建了一个实例对象，并且包含了原型对象（Object）上面的所有方法. 比如说:

```js
 obj.toString();

 //转换字符串的方法
```

总结一下，原型模式其实就是，提供一些列共有的属性和方法 的模式。来个栗子:

```js
 function Person(){

 }

 Person.prototype.name="jimmy";

 Person.prototype.gender="male",

 Person.prototype.outName = function(){

     console.log(this.name);

 }

 var jimmy = new Person();

 jimmy.outName;

 //jimmy
```

好难看啊~

看只是表面，我们来说说内在。 原型模式的内涵超级丰富。 在原型模式中，新增了一个东西prototype(原型), 这就是原型模式的精华。

上面说了构造函数新增了constructor作为表示符，原型模式里面有prototype作为顶梁柱。

prototype(原型)就是一个指针，用来指向一个对象，并且这个对象是所有实例共有的原型对象。

而这个原型模式和构造函数模式的最重要的一个却别就是。this指针的指向问题。

使用构造函数模式，这个this指针指向的是构造函数里面的this。

而使用原型模式,这个this是指向 原型对象(prototype);

不理解啊~ 待我细细解释。

### 细说原型模式

其实创建原型的方法也是使用构造函数，只是每个函数创建的时候会自带一个prototype属性指向函数的原型对象。而且prototype上还有一个constructor用来指向 该构造函数中的this. 这样， 原型对象上面就有了和 构造函数的连接。
我们可以检验一下:

```js
 //同样一个例子

 function Person(){

 }

 Person.prototype.name="jimmy";

 Person.prototype.gender="male",

 Person.prototype.outName = function(){

     console.log(this.name);

 }

 var jimmy = new Person();

 console.log(jimmy.constructor);

 //Person

 console.log(Person.prototype.constructor);

 //Person
```

现在我们只要知道Person.prototype就是代表Person的原型，that's enough~
关于__proto__,prototype,constructor后面我会专门独立出来说明的。
留个坑，写完了放一个链接.
说了这么多，蒙蒙哒~

来，总结一下实例与构造函数模式以及原型模式之间的联系:

|原型模式     |实例<>构造函数的原型对象   |
|------------| --------------------- |
|构造函数模式: |实例<>构造函数           |

这样应该差不多能理解吧。 尽力了~
但是，一门高级的语言不会让你知道你不知道。
实际上，使用原型的坑也是很多的。重用性解决了，但是自定义却不能实现了。
然而这只是一小部分，给你来个大的。

原型模式之引用类型的坑

```js
 function Person(){}

 Person.prototype.wife = {

     "Sheily":{

         age:23,

         height:170

     },

     "Adel":{

         age:20,

         height:172

     }

 }

 var jimmy = new Person();

 console.log(jimmy.wife);

 //两个人

 var sam = new Person();

 sam.wife.angela = {

     age:24,

     height:18

 }

 console.log(sam.wife);

 //3个人

 console.log(jimmy);

 //3个人
```

这样就能看出明显的区别。 想想吧，老婆能是别人的吗?

聪明的社区 scratch their butt。 然后想到，构造函数模式和原型模式一起用不久over了吗？

真聪明. 取个名字吧，混合模式~(自己编的 :)

### 混合模式

通过上例的说明，相信大家应该能理解我所说的，原型模式 is not versatile.
所谓的混合就是将两者结合在一起:
构造函数定义的是实例的属性，而原型模式定义的是共享的属性(prototype).
来个栗子:

```js
 function Person(name,wife){

     this.name = name;

     this.wife = wife;

 }

 Person.prototype.myWife = function(){

     console.log(this.wife);

 }

 var jimmy = new Person("jimmy",{

     sheriy:{

         age:19,

         height:180

     }

 });



 jimmy.myWife();

 //sheriy

 var sam = new Person('sam',{

     angela:{

         age:20,

         height:190

     }

 })

 sam.myWife();

 // angela
```

再也不用担心自己的老婆是别人家的了。这种方式，应该是js社区极力推崇的。但是我们写项目的时候，说实话，真心用不到(因为我们的项目太小了)。所以在这里推荐一个，我自己经常使用的。

### 字面量模式

直接上栗子:

```js
 var login = {

     userName : document.querySelector('#userName'),

     ps: document.querySelector('#password'),

     loginBtn: document.querySelector('#loginBtn'),

     init:function(){

         var _this = this;

         this.loginBtn.addEventListener("click",function(){

             var name = _this.userName.getAttribute("value"),

                 ps = _this.ps.getAttribute('value');

             http.ajax({

                 url:Pathurl.login,

                 dataType:'JSON',

                 contentType:"application/json"

             })

             .then(function(data){

             //请求成功后的相关操作

             })

         },false);

     }

 }

 login.init();
```

这应该算是一个基本的写法吧。 本人青睐于这种写法的原因是，他有一个入口函数，init()，而且获取元素后，可以重复性的使用，方便你每次添加元素的时候重新获取.

------

本文转载自一个很喜欢的微信公共号：前端圈

发现很多好文章都停留在了微信公共号里面，你不关注的情况下是看不到这些文章的。这样一定角度上就失去了网络自由而公开的属性，信息变成了一个个孤岛，偶尔也会思考，越做越大的微信巨头是不是也在割裂着互联网本身最重要的公开自由的基因。

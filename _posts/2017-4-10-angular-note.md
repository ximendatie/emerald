---
title: AngularJS 学习笔记
---

这是根据个人学习做的AngularJS笔记，主要是自己的学习难点记录和内容归纳，不具有系统性，大家可以选择性阅读。

<!--more-->


## AngularJS简介

AngularJS这个js框架可以概括的说做了两件事：

1. 通过`指令`扩展了HTML  
2. 通过`表达式`绑定数据到HTML  

> AngularJS有人说是MVC框架，也有人说是MVVM，其实官方说法它是一个MVW框架，即Model-View-Whatever，Whatever很霸气，把什么都包括了。



## 指令

    AngularJS 通过被称为 指令 的新属性来扩展 HTML。
    AngularJS 通过内置的指令来为应用添加功能。
    AngularJS 允许你自定义指令。

1. ng-app 指令定义了 AngularJS 应用程序的 根元素。
    ng-app 指令在网页加载完毕时会自动引导（自动初始化）应用程序。
2. ng-init 指令为 AngularJS 应用程序定义了 初始值。
    通常情况下，不使用 ng-init。您将使用一个控制器或模块来代替它。
3. ng-model 指令 绑定 HTML 元素 到应用程序数据。
    ng-model 指令也可以：
    为应用程序数据提供类型验证（number、email、required）。
    为应用程序数据提供状态（invalid、dirty、touched、error）。
    绑定 HTML 元素到 HTML 表单。    
    为 HTML 元素提供 CSS 类。
    ng-model 指令根据表单域的状态添加/移除以下类：
    ng-empty
    ng-not-empty
    ng-touched
    ng-untouched
    ng-valid
    ng-invalid
    ng-dirty
    ng-pending
    ng-pristine
4. ng-repeat 指令对于集合中（数组中）的每个项会 克隆一次 HTML 元素。
5. ng-controller 控制器 控制 AngularJS 应用程序的数据。


## Scope作用域

Scope(作用域) 是应用在 HTML (视图) 和 JavaScript (控制器)之间的纽带。
Scope 是一个对象，有可用的方法和属性。
Scope 可应用在视图和控制器上。

## 其他

一个应用可以有多个控制器
$rootScope 可作用于整个应用中。是各个 controller 中 scope 的桥梁。用 rootscope 定义的值，可以在各个 controller 中使用。

## AngularJS的优缺点

优点：

1. AngularJS模板功能强大丰富，自带了极其丰富的angular指令。
2. AngularJS是完全可扩展的，与其他库的兼容效果很好，每一个功能可以修改或更换，以满足开发者独特的开发流程和功能的需求。
3. AngularJS是一个比较完善的前端MVC框架，包含服务，模板，数据双向绑定，模块化，路由，过滤器，依赖注入等所有功能；
4. AngularJS是互联网巨人谷歌开发，这也意味着他有一个坚实的基础和社区支持。

缺点：

1. AngularJS强约束导致学习成本较高，对前端不友好。但遵守 AngularJS 的约定时，生产力会很高，对 Java 程序员友好。
2. AngularJS不利于SEO，因为所有内容都是动态获取并渲染生成的，搜索引擎没法爬取。

性能问题：AngularJS作为 MVVM 框架，因为实现了数据的双向绑定，对于大数组、复杂对象会存在性能问题。

### 参考

http://www.w3cschool.cn/



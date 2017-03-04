---
title: 什么是webpack
---

这篇是之前为了组内技术分享的时候准备的，虽然那个时候讲的不太好，现在贴出来告诉自己以后要做的更好


## 现状
- - - -
当今越来越多的网站已经从网页模式进化到了 Webapp 模式。它们运行在现代的高级浏览器里，使用各种更新的技术来开发，网页已经不仅仅是完成浏览的基本需求，并且webapp通常是一个单页面应用，每一个视图通过异步的方式加载，这导致页面初始化和使用过程中会加载越来越多的 JavaScript 代码。

<!--more-->

前端开发和其他开发工作的主要区别，首先是前端是基于多语言、多层次的编码和组织工作，其次前端产品的交付是基于浏览器，这些资源是通过增量加载的方式运行到浏览器端，如何在开发环境组织好这些碎片化的代码和资源，并且保证他们在浏览器端快速、优雅的加载和更新，就需要一个模块化系统，这个理想中的模块化系统是前端工程师多年来一直探索的难题。



它们拥有着复杂的JavaScript代码和一大堆依赖包。为了简化开发的复杂度，前端社区涌现出了很多好的实践方法

* 模块化，让我们可以把复杂的程序细化为小的文件;
* 类似于TypeScript这种在JavaScript基础上拓展的开发语言：使我们能够实现目前版本的JavaScript不能直接使用的特性，并且之后还能能装换为JavaScript文件使浏览器可以识别；
* Scss，less等CSS预处理器

## 模块系统
- - - -
模块系统主要解决模块的定义、依赖和导出，先来看看已经存在的模块系统。

```
<script src="module1.js"></script>
<script src="module2.js"></script>
<script src="libraryA.js"></script>
<script src="module3.js"></script>
```

这是最原始的 JavaScript 文件加载方式，如果把每一个文件看做是一个模块，那么他们的接口通常是暴露在全局作用域下，也就是定义在 window 对象中，不同模块的接口调用都是一个作用域中，一些复杂的框架，会使用命名空间的概念来组织这些模块的接口，典型的例子如 YUI 库。

这种原始的加载方式暴露了一些显而易见的弊端：

* 全局作用域下容易造成变量冲突
* 文件只能按照 < script > 的书写顺序进行加载
* 开发人员必须主观解决模块和代码库的依赖关系
* 在大型项目中各种资源难以管理，长期积累的问题导致代码库混乱不堪

这时候就出现了一些模块化的解决方案

### commonJS
require（”vue”）
require(“../file.js”)
module.exports = something

优点：
服务器端模块便于重用
NPM 中已经有将近20万个可以使用模块包
简单并容易使用

缺点：
同步的模块加载方式不适合在浏览器环境中，同步意味着阻塞加载，浏览器资源是异步加载的
不能非阻塞的并行加载多个模块

### AMD
define(“jq”, “lodash”, funciton (d1, d2) {
})
Requirejs
优点：

适合在浏览器环境中异步加载模块
可以并行加载多个模块
缺点：

提高了开发成本，代码的阅读和书写比较困难，模块定义方式的语义不顺畅
不符合通用的模块化思维方式，是一种妥协的实现

### CMD
define(function(require, export, module) {
	var $ = require('jquery');
  	var Spinning = require('./spinning');
	exports.doSomething = ...
  	module.exports = ...
})

优点：
依赖就近，延迟执行
可以很容易在 Node.js 中运行
缺点：

依赖 SPM 打包，模块的加载逻辑偏重

### UMD
(function (root, factory) {
    if (typeof define === 'function' && define.amd) {
        // AMD. Register as an anonymous module.
        define(['exports', 'b'], factory);
    } else if (typeof exports === 'object' && typeof exports.nodeName !== 'string') {
        // CommonJS
        factory(exports, require('b'));
    } else {
        // Browser globals
        factory((root.commonJsStrict = {}), root.b);
    }
}(this, function (exports, b) {
    //use b in some fashion.

    // attach properties to the exports object to define
    // the exported module properties.
    exports.action = function () {};
}));


### ES6 模块
import "jquery";
export function doStuff() {}

优点：
容易进行静态分析
面向未来的 EcmaScript 标准

缺点：
原生浏览器端还没有实现该标准
全新的命令字，新版的 Node.js才支持

具体实现的话：babel

### 我们期待的模块系统
可以兼容多种模块风格，尽量可以利用已有的代码，不仅仅只是 JavaScript 模块化，还有 CSS、图片、字体等资源也需要模块化。

前端模块加载

### 前端模块加载

前端模块要在客户端中执行，所以他们需要增量加载到浏览器中。

模块的加载和传输，我们首先能想到两种极端的方式，一种是每个模块文件都单独请求，另一种是把所有模块打包成一个文件然后只请求一次。显而易见，每个模块都发起单独的请求造成了请求次数过多，导致应用启动速度慢；一次请求加载所有模块导致流量浪费、初始化过程慢。这两种方式都不是好的解决方案，它们过于简单粗暴。

分块传输，按需进行懒加载，在实际用到某些模块的时候再增量更新，才是较为合理的模块加载方案。

要实现模块的按需加载，就需要一个对整个代码库中的模块进行静态分析、编译打包的过程。

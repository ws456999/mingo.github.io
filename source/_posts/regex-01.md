---
title: 正则表达式（一）
tags: 正则
---

常年没有写过正则了，今天来好好学习一波，有点长，留着点东西准备些下一篇

先放一波格式化代码
<!--more-->
``` javascript
// 正则添加千分位
//参数说明：num 要格式化的数字 n 保留小数位
function formatNum(num,n){
  num = String(num.toFixed(n))
  var re = /(-?\d+)(\d{3})/
  while(re.test(num)) {
    num = num.replace(re,"$1,$2")
  }
  return num
}
```

## 贪婪模式跟非贪婪模式
``` javascript
var str = "aaab",
reg1 = /a+/, //贪婪模式
reg2 = /a+?/;//非贪婪模式
console.log(str.match(reg1)); //["aaa"], 由于是贪婪模式, 捕获了所有的a
console.log(str.match(regs)); //["a"], 由于是非贪婪模式, 只捕获到第一个a
(\+86)?1\d{10}
```

实际上, 非贪婪模式非常有效, 特别是当匹配html标签时. 比如匹配一个配对出现的div, 方案一可能会匹配到很多的div标签对, 而方案二则只会匹配一个div标签对.

``` javascript
var str = "<div class='v1'><div class='v2'>test</div><input type='text'/></div>";
var reg1 = /<div.*<\/div>/; //方案一,贪婪匹配
var reg2 = /<div.*?<\/div>/;//方案二,非贪婪匹配
console.log(str.match(reg1));//"<div class='v1'><div class='v2'>test</div><input type='text'/></div>"
console.log(str.match(reg2));//"<div class='v1'><div class='v2'>test</div>"
```

## 区间量词的非贪婪模式

一般情况下, 非贪婪模式, 我们使用的是`*?`, 或 `+?` 这种形式, 还有一种是 `{n,m}?`.
区间量词`{n,m}` 也是匹配优先, 虽有匹配次数上限, 但是在到达上限之前, 它依然是尽可能多的匹配, 而`{n,m}?` 则表示在区间范围内, 尽可能少的匹配.

需要注意的是:

- 能达到同样匹配结果的贪婪与非贪婪模式, 通常是贪婪模式的匹配效率较高.
- 所有的非贪婪模式, 都可以通过修改量词修饰的子表达式, 转换为贪婪模式.
- 贪婪模式可以与固化分组(后面会讲到)结合，提升匹配效率，而非贪婪模式却不可以.

## 分组

正则的分组主要通过小括号来实现, 括号包裹的子表达式作为一个分组, 括号后可以紧跟限定词表示重复次数. 如下, 小括号内包裹的abc便是一个分组:

``` javascript
/(abc)+/.test("abc123") == true
```

那么分组有什么用呢? 一般来说, 分组是为了方便的表示重复次数, 除此之外, 还有一个作用就是用于捕获, 请往下看.

## 捕获性分组

捕获性分组, 通常由一对小括号加上子表达式组成. 捕获性分组会创建反向引用, 每个反向引用都由一个编号或名称来标识, js中主要是通过 `$+` 编号 或者 +编号 表示法进行引用. 如下便是一个捕获性分组的例子.

``` javascript
var color = "#808080";
var output = color.replace(/#(\d+)/,"$1"+"~~");//自然也可以写成 "$1~~"
console.log(RegExp.$1);//808080
console.log(output);//808080~~
```

以上, `(\d+)` 表示一个捕获性分组, `RegExp.\$1` 指向该分组捕获的内容. `$+编号` 这种引用通常在正则表达式之外使用. `+编号` 这种引用却可以在正则表达式中使用, 可用于匹配不同位置相同部分的子串.

``` javascript
var url = "www.google.google.com";
var re = /([a-z]+)\.\1/; // ps：前面（）中使用的是贪婪模式
console.log(url.replace(re,"$1"));//"www.google.com"
console.log(RegExp.$1) // "google"
```

## 非捕获性分组

非捕获性分组, 通常由一对括号加上`?:`加上子表达式组成, 非捕获性分组不会创建反向引用, 就好像没有括号一样. 如下:

``` javascript
var color = "#808080";
var output = color.replace(/#(?:\d+)/,"$1"+"~~");
console.log(RegExp.$1);//""
console.log(output);//$1~~
```

以上, `(?:\d+)` 表示一个非捕获性分组, 由于分组不捕获任何内容, 所以, `$1` 指向空字符串.
实际上, 捕获性分组和无捕获性分组在搜索效率方面也没什么不同, 没有哪一个比另一个更快.

## 命名分组

语法: (?…)

命名分组也是捕获性分组, 它将匹配的字符串捕获到一个组名称或编号名称中, 在获得匹配结果后, 可通过分组名进行获取.（ javaScript 中不支持命名分组 ）

## 固化分组


固化分组, 又叫原子组.

语法: (?>…)

如上所述, 我们在使用非贪婪模式时, 匹配过程中可能会进行多次的回溯, 回溯越多, 正则表达式的运行效率就越低. 而固化分组就是用来减少回溯次数的.（ javaScript 中不支持固话分组 ）


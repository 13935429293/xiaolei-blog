---
title: 正则API
date: 2016-11-06 15:44:31
categories: javascript
tags: javascript
---
### 创建正则表达式对象

*   创建正则表达式对象的方式:
    +   var reg=new RegExp(/正则表达式/);
    +   var reg=/正则表达式/;
    +   /正则表达式/.方法名(参数);
*   正则表达式就是一个字符串
*   通用字符.(除了换行)
*   []匹配一个字符
*   逻辑匹配符：|  优先级最低
*   （）调整优先级
*   任意个字符*（紧跟在他前面的那个字符出现0或多次）
*   任意个字符+（紧跟在他前面的那个字符出现1或多次）
*   ?（紧跟在他前面的那个字符出现0|1）
*   {n}n次重复
*   {b,}至少n次
*   {n,m}至少出现n~m次
*   ^以什么开始[^]表示否定不出现
*   $以什么结束
*   \d 数字 \D非数字
*   \s空字符（空格、Tab、回车） \S非空字符
*   \w文字（除了标点符号以外的）  \W非文字

### 实例方法
*   是否匹配上
*   将匹配上的东西显示出来
*   将匹配上的东西替换成其他

var patt=new RegExp(pattern,modifiers)

####    test()

RegExp.prototype.test(str);
```js
var patt1=new RegExp("e");
document.write(patt1.test("The best things in life are free"));
```
####    exec()
RegExp.prototype.exec(str)
+   为指定的一段字符串str执行搜索匹配操作。它的返回值是一个数组或者 null。
+   返回的数组包括匹配的字符串作为第一个元素，紧接着一个元素对应一个成功匹配被捕获的字符串的捕获括号;
+   如果正则模式是一个空字符串，则exec方法会匹配成功，但返回的也是空字符串。
+   整个数组的length属性等于捕获匹配的数量再加1。


### 支持正则表达式的 String 对象的方法
字符串对象的方法：正则对象被作为参数，如 str.match(regex)。
此时str.match(regex)方法会一次性返回所有匹配成功的结果.

*   g表示的是全局
*   i 表示的是小写的
    str=str.replace(/帅/g,"猥琐");
    console.log(str);
    

### 匹配中文

*   /[\u4e00-\u9fa5]/



---
title: node常见问题
date: 2015-8-25
categories: Node
tags: Node
---

```js
 给`exports`
 赋值不管用
 但是下面这种做法就可以
 `exports = module.exports = _;`
 下面这种做法的目的:可能在别的地方更改了
 exports
 的引用
 让`exports = module.exports` 
 的目的就是让两者重新建立引用关系
```

*   javaScript 书写标准 standerdjs.com
*  node操作模板拼接字符串，会编码输出 
```js
    // 1. 调用 template.compile 方法，传入一个模板字符串，得到一个渲染函数
    //# 表示不编码输出
    const render = template.compile(`
    <h1>{{# title }}</h1>
    `)
    
    // 防止用户恶意注入 JavaScript 代码
    const result = render({
      title: '<span>hello</span>'
    })
    console.log(result)
```
*   正则表达是匹配问题
    -   正则分组之后，如果匹配成功，则会得到一个数组
    -   数组中第 0 项永远是要匹配的字符串自身
    -   从 1 项开始，数正则表达式中的括号就可以了（数左括号）
    -   左括号是第几个，则解析出来的数据数组中下标为该数字的元素
*   markdown文件直接转html文件的时候，会出现在浏览器上看html文件的时候会有乱码的情况，
    原因是有两种情况：1、如果是通过http传输他会通过content-type来看他的那个字符编码是
    什么格式，2、如果是本地转换他会看html里面的mate charset 来看里面的字符编码格式。
    直接将md转为html的时候，很有可能是因为没有写mate charset标签的字符编码，所以会出现乱码
*   http解析数据
    -   当客户端浏览器收到你发送的响应数据的时候
    -   浏览器会先查看响应报文头中的 Content-Type 中的 charset 编码，然后根据该编码解析数据
    -   如果响应报文头中没有 Content-Type 那么浏览器则根据 HTML 结构中的 <meta charset="UTF-8"> 来解析数据
    
*   对markdown转换成html时，在引用模板写入html结构的时候会出现只是将html转换成字符串没有转变成html结构的情况，是因为有"<"的原因浏览器会将"<"进行转换，这时需要在模板中加`{{body}}`，意为不进行编码解析,防止用户注入恶意的javascript代码
*   操作文件的路径使用注意事项：
    -   如果是以 / 开头的路径，则就是去执行当前脚本所属的磁盘根路径去找
    -   如果是以 C:/dev/nvm/settings.txt ，则直接去找该绝对路径
    -   如果是以 ./ 或者 ../ 开头的，则是相对于执行 node 命令的时候所处的路径
    -   所以说，如果是操作相对路径的文件，最好把相对路径转为绝对路径
    -   但是绝对路径又不能写死
    -   可以使用每一个文件模块中都提供了两个属性：__dirname 和 __filename
    -   __dirname 用来获取当前文件模块所属目录的绝对路径
    -   __filename 用来获取当前文件的绝对路径，这个属性用的比较少
    -   注意：加载自己写的相对路径模块不受执行 node 命令所处目录影响
    -   也就是说，加载文件模块还是使用相对路径
      
```js
//  C:\Users\iroc\Desktop> node code/02_文件路径.js
//     该文件中的相对路径是去执行node命令的目录地方去找了 'C:\Users\iroc\Desktop\README.md
//  C:\Users\iroc> node .\Desktop\code\02_文件路径.js
//     C:\Users\iroc\README.md
//  想要解决上面的问题：
//     每一个模块中都提供了两个属性：__dirname 和 __filename
```      
*   表单 GET 提交：（表单的 method 属性默认就是 get）：
    -   表单会将表单中所有具有 name 属性的 input 按照
    -     name=input-value&name1=input-value2&xxx=xxx
    -   拼接完之后，找到自己的 action（就是请求路径）
    -   然后在 action 之后 加一个 ? ，后面跟上拼接好的查询字符串
    -   最后，发起请求
    -   albumName=xxx&xxx=xxx
    -   /album/add?albumName=xxx&xxx=xxx
*   有文件的表单提交
    
    -   请求方法必须是post提交
    -   将表单的enctype设置为multipart/form-data(得到的是二进制数据)
*   编码encodeURI(要编码的字符串)
    解码decodeURI（要解码的字符串）
*   post提交不含文件的表单，后台解析字符串是通过什么方式进行？
    
    可以引入querystring,核心模块，通过queryString.parse(字符串)得到的就是一个字符
    串对象
      
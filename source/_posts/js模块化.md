---
title: js模块化
date: 2014-8-25
categories: javascrpit
tags: javascrpit
---


---------------------------------------------------------

# 当你的网站开发越来越复杂的时候，会经常遇到什么问题？

  *  恼人的命名冲突
  *  繁琐的文件依赖
  *  Sea.js 可以解决命名空间污染、文件依赖的问题。

     历史上，JavaScript一直没有模块（module）体系，
     无法将一个大程序拆分成互相依赖的小文件，再用简单的方法拼装起来。 其他语言都有这项功能，
    比如Ruby的 require、Python的 import ，
    甚至就连CSS都有 @import ， 但是JavaScript任何这方面的支持都没有，这对开发大型的、复杂的项目形成了巨大障碍。

   *    什么是模块化？
        -   模块化是指解决一个复杂问题时自顶向下逐层把系统划分成若干模块的过程，有多种属性，分别反映其内部特性。
        -   解决复杂问题的一种方式而已
        -   电脑：CPU、主板、显示器、内存、硬盘、输入与输出设备
        -   房屋模块化
   *    使用模块化开发的方式带来的好处？
        -   生产效率高
        -   可维护性高
        -   软件的声明周期中最长的阶段其实并不是开发阶段, 而是维护阶段，需求变更比较频繁，使用模块化的开发 方式更容易维护

# 模块化开发演变过程

## 		模块化
   *    以后如果不使用第三方规范的情况下，如果写模块可以采用下面这种方式：

        ```js
          1. 分号是什么意思
           匿名自执行函数加分号目的是为了防止前边的代码没有加分号造成语法解析错误

          2. 为什么要给你的代码加一个匿名自执行函数
            给代码加一个匿名自执行函数，为了避免全局作用域污染，核心是利用函数的私有作用域

          3. 为什么要把使用的依赖作为参数传递进来
         使用的依赖作为参数传递进来母的为了减少作用域查找机制，提高代码执行效率

         ;(function (形参模块名, 依赖项, 依赖项) {
           // 通过 形参模块名 修改模块

           window.模块名 = 形参模块名
         })(window.模块名 || {}, 依赖项, 依赖项)
         ```
## 		seajs

   *   在 SeaJS 中，一个 js 脚本文件就是一个模块

        -    模块具有两个特性：
        模块要有一个私有作用域：避免全局命名空间污染
        模块可以向外导出内部成员，供别的模块加载和使用
        所以：只要使用了 SeaJS ，那所有的 js 文件都通过 define 函数去定义该模块
        并且将所有的模块代码写到 define 定义的回调函数中
        define 方法的回调函数中分别传递三个参数：
        require
        exports
        modules
   *    每一个模块中有一个 require 函数可以用来加载指定模块需要接收一个模块路径

        -   加载指定模块并且执行该模块中的代码
        -   得到该模块中的暴露的接口对象：module.exports

   *    模块的作用域和导出

        -   模块天生就是一个私有作用域，在该模块内部定义的所有成员，默认都是该模块私有的
        -   如果模块内部的成员想要被外部所访问：

            必须通过使用 module.exports 对象向外暴露
            require 函数加载模块的时候，会自动拿到模块内部的 module.exports 对象

## 		seajs 执行过程

   *    首先需要在浏览器页面中引包，即引入seajs
   ```js
   <script src="seajs路径"></script>
   ```
   *    调用seajs.use方法
   ```js
   seajs.use(可以引起加载整个模块化的主要就是文件路径，其后缀js可以省略)；
   ```
   *    加载的模块化js需要写在defind函数中
   ```js
   define(function(require,exports,module){
        require(暴露接口的路径)返回值为暴露接口对象
        代码块
   })
   ```
   *    如若想要实现某个功能，且某个功能代码量比较大，可以将该功能写入模块中，通过module.exports
   暴露接口
   ```js
   define(function(require,exports,module){
        function add(x,y){
            return parseFloat(x)+parseFloat(y);
        }

        module.exports.add = add;
   })

   ```

## 		exports与module.exports联系与区别
   *    每个文件模块中默认的对外的接口对象是module.exports
   同时seajs还提供了一个接口对象exports

   注意：

   exports 是module.exports接口对象的一个引用，也就是说
   修改了exports相当于修改了module.exports，但是，如果想要向外
   暴露一个单独的变量、函数等成员，那就必须通过给module.exports
   赋值才可以
   ```js
   define(function (require, exports, module) {
     // 每一个模块内部相当于有这么一句代码
     // var exports = module.exports

     exports.add = require('./add')
     exports.sub = require('./sub')

     // 给 exports 赋值，就切断了和 moudle.exports 之间的引用关系
     // 原来的 module.exports 接口对象不受影响
     // 所以：在模块内部给 exports 赋值不管用
     // exports = function () {
     //   console.log('test')
     // }

     // 每一个函数内部相当于有这么一句代码
     // return module.exports
   })

   ```
   
   









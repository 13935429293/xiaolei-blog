---
title: Node中的javascript
date: 2015-11-18
categories: Node
tags: Node
---
##  Node运行环境
   *  REPL运行环境
        -   read:读取
        -   eval：执行
        -   print：输出
        -   LOOP：循环
        -   在终端输入`node+enter`，则进入REPL环境
        -   `Ctrl+cc`，退出REPL环境
*   `kangax.github.io`//查看ES6支持版本
##  全局对象
### globle
   * 相对于浏览器的全局对象window
   * 全局作用域和模块作用域的概念
### console
   * log
   * info
   * error
   * warn


### buffer

   *    全局构造函数，也可以说是一个全局对象
   *    专门用来解析二进制数据的
   *    buffer类似于数组，二进制数据的一种表现形式
   *    数组内部是一个由 16进制 表现形式的一个一个的元素，每个元素其实就是一个字节
        一个字节就是 8位
### 全局函数
   - 在 Node 环境中，也提供了 定时器对象，和浏览器中的定时器用法一致，但是实现方式有差异
   - setInterval 和 clearInterval
   - setTimeout 和 clearTimeout
   - setImmediate 和 clearImmediate
      + 这也是一个定时器，但是不需要时间
        可以异步的执行一段代码
        类似于 `setTimeout(function () {}, 0)`

### Node中的模块成员

   - dirname 和 filename
     + 这是两个属性，每一个模块内部都可以直接使用
   - require
     + 加载模块，执行模块中的代码，得到该模块中的 module.exports 接口对象
   - exports
     + 它是 module.exports 的一个引用
   - module
     + 模块对象，其中有一个非常重要的属性:module.exports
     + 每一个模块内部对外暴露的成员就是 module.exports

### 模块系统

   - 如何定义模块成员
   - 模块成员之间如何进行通信
   #### commomJS

    commonJS 包含模块定义规范，还给javaScript语言没有触及到服务器领域制定了一些规范
    比如：二进制数据操作、文件操作、网络操作、进程操作、？模块系统规范。。。

   *    每一个JavaScript脚本就是一个模块，每一个模块都是一个私有作用域
   *    通过require加载模块
   *    通过module.exports 暴露接口
   #### Node的模块形式（三种）
   *    文件模块

    以 ./ 或 ../ 开头的模块标识就是文件模块，一般就是用户编写的。

   *    核心模块
        -   核心模块就是 node 内置的模块，需要通过唯一的标识名称来进行获取。
        -   每一个核心模块基本上都是暴露了一个对象，里面包含一些方法供我们使用
        -   一般在加载核心模块的时候，变量的起名最好就和核心模块的标识名同名即可
        -   核心模块本质上也是文件模块
            +   核心模块已经被编译到了 node 的可执行程序，一般看不到
            +   可以通过查看 node 的源码看到核心模块文件
            +   核心模块也是基于 CommonJS 模块规范
        -    常用的核心模块


                    模块名称          作用

                    fs               文件操作

                    http             http服务

                    net              Socket网络编程

                    os               操作系统相关

                    path             路径操作

                    querystring      处理查询字符串  |

                    url              解析处理url路径

                    util             工具函数
   *    第三方模块
        -   只能在 Node 中使用的包
        例如：open fs-extra
        -   只能在浏览器中使用的包
        例如 jquery swiper
        -   既能在浏览器也能在Node中使用的包
        不涉及具体的环境 API
        例如 moment underscore


##  模块的加载流程

   -   优先从缓存加载(如果缓存中已经存在，就不会在去加载);
   -   模块标识符分析
        +   扩展名分析
            *   .js
                通过fs文件同步读取js文件并编译执行
            *   .node
                通过C/C++进行编写的Addon。通过dlopen方法进行加载。
            *   .json
                -   读取文件调用JSON.parse解析加载
                -
        +   文件模块
        +   核心模块
        +   第三方模块
            *   先看一下是不是文件模块，再看一下是不是核心模块
                不可能有第三方模块和核心模块重名
            *    npm install 包名 的时候的这个包的名称，就可以通过 require 函数指定加载
            *    对于第三方包，Node 会直接进入当前 app.js 同级目录下的 node_modules 中找该模块名同名的目录
            *    如果找到，找 该模块名同名的目录中的 package.json 文件
            *    如果找到该文件，则解析出来该文件，找里面的 main 属性
            *    如果 main 中指定的文件模块存在，则直接加载
            *    如果main中指定的文件模块没有扩展名
            *    .js .node .json
            *    如果没有找到 moment 中的 package.json 文件
            *    或者找到package.json 文件，但是没有 main 属性
            *    或者找打 mian 属性，但是指定的文件模块不存在
            *    这种情况下，node 会按照 index.js index.node index.json 的方式去查找
            *    如果压根儿就没有 moment 这个目录，那这个时候进入上一级目录下的 node_modules 查找
            *    当在上一级 node_modules 目录下找到 moment 目录的时候，查找规则同上
            *    如果上一级还找不到，则继续向上查找，规则同上
            *    直到一直到达当前文件 app.js 所属盘符根路径，如果还找不到，则报错：can not find module

##  包与npm
    
    包其实就是一些模块组织到一起，放到一个目录中的一个称呼，叫包或者模块都行。
##  package.json
    
    每个项目的根目录下面，一般都有一个package.json文件， 定义了这个项目所需要的各种模块，以及项目的配置信息（比如名称、版本、依赖项等信息）。
    
    package.json文件内部就是一个JSON对象， 该对象的每一个成员就是当前项目的一项设置。 比如name就是项目名称，version是版本。
    
   -   npm init [-y]
   
    生成 package.json 文件
   -   main
   
    用来指定加载的时候的入口模块的
   -   dependencies
   
    当前项目或者说包的依赖项
    npm install 的时候，会自动查找当前目录下的 package.json 文件中的 dependencies 属性
    然后依次下载
    
##   本地安装与全局安装
   -   全局安装
        +   本质上也是一个包
        +   只不过提供了一个命令的使用形式
        +   可以在任意目录中使用该命令工具辅助完成一些功能
        +   这个工具不能通过代码调用
        +   这些工具就是类似于 nvm、nrm、lessc、browser-sync 等工具
   -   本地安装
        +   对于想要使用第三方包的功能，就必须下载到本地项目中， 想要使用第三方包暴露的接口 API 辅助实现功能。
##  npm命令操作

   -   npm help：查看 npm 命令列表
   -   npm -l：查看各个命名的简单用法
   -   npm -v：查看 npm 的版本
   -   npm init [-y]：初始化一个 package.json 文件
   -   npm info 包名 [字段名]：查看指定模块的 package.json 信息
   -   npm search 包名：该命令用于搜索 npm 仓库
   -   npm list：以树型结构列出当前项目安装的所有模块，以及它们依赖的模块
   -   npm list 包名：列出单个模块
   -   npm list -g：列出全局安装的模块
   -   npm install [--save] 包名[#版本号]
        +   npm install 包名：安装包到当前项目下的 node_modules 目录下
        +   npm install|i --save|-S 包名 安装包的同时把依赖项保存到 包说明文件中
            本地项目安装：目的是为了辅助你的代码功能开发
   -   npm uninstall [--save] 包名
        +   npm uninstall 包名：删除包，但是如果包说明文件中有依赖项，那么不会删除
        +   npm uninstall --save 包名：删除包，同时将 package.json 文件中的依赖项也删除
   -    npm install
   -    npm install -g 包名
        全局安装：一般用于安装命令行中的 CLI 工具
   -    npm uninstall -g 包名
   -    npm config
        +   npm config list 查看 npm 配置项
        +   npm config set init.author.name $name 使用 npm init 时，默认的 name
        +   npm config set init.author.email $email 使用 npm init 时，默认的 email
        +   npm congig set prefix "路径" 改变全局包安装路径
        +   npm config set registry "镜像路径"     
##  path
   *    path.dirname(path)//获取一个路径的目录部分
   *    path.isAbsolute()//以`/`和`c:/`判断是不是绝对路径
   *    path.join()//将多个参数中间以`\`的形式将路径进行拼接
        将多个路径拼接为一个完整的路径
   ``  js
    path.join()
   ``
   *    path.parse()//将路径当中的任意部分转换为对象
   *    path.format（{}）将路径对象转换为路径，与path.parse（）方法相反
   *    path.sep（）获取操作系统路径分隔符
        -   应用场景：多平台转入
   *    path.solve（）//将多个路径拼接为绝对路径
   *    path.normalize()//将非标准路径转换为标准路径
##  文件操作
   ###  同步调用和异步调用

   只有fs模块对文件的几乎所有操作都有同步和异步两种形式，例如：readFile() 和 readFileSync()。
   *    同步读取文件` readFileSync()`
        -   同步代码会阻塞后续代码的执行，效率不高
        -   同步API需要通过try-catch来捕获异常
        -   同步执行文件比较符合程序员的逻辑，比较简单,就是所有任务都是自己干，而且是一步一步往下执行
   *    异步读取文件
        -   异步API往往会伴随着一个回调函数用来接收返回值或者处理异常
        -   回调函数的参数中第一个参数一般都是err对象，用来判定异步API是否异常
        -   异步API不能通过try-catch来捕获异常
        -   一般文件操作中，所有的异步API，都会在回调函数中提供了一个 error 对象
            如果操作过程有异常，则 error 是一个异常对象
            如果操作成功，没有问题，则 error 是一个 null
            所以，为了判定是否有异常，if(err) { // 处理异常 }
            ```js
            try {
              fs.readFile('./data/', 'utf8', (err, cai) => {
                console.log(cai)
              })
            } catch (e) {
              console.log('菜卖没了')
            }
            执行结果为undefined;
            ```
        -   开发阶段，使用 throw err 的形式抛出异常
            目的是为了快速的定位代码的错误
            如果是网站服务器中，这个就不会去 throw err，一般会有异常处理机制
            一般在生产环境，会处理异常，例如记录日志方便排查错误
            throw err 会直接抛出异常，退出进程
        ```js
        fs.readFile('./data/', 'utf8', (err, cai) => {
          if (err) {
            throw err
          }
          console.log(cai)
        })
       ```

       ** 文件乱码的原因是，指定的是用utf-8解析，但是文件格式是gbk,所以出现的中文文字是乱码 **
### node支持的编码类型
   *    ascii
   *    utf8
   *    万国码
   *    utf16le
   *    ucs2
   *    base64
   *    图片编码
        ```js
        //<img src="data:image/wenjianggeshi(forexample jpg);base64,base64 编码">
        ```
   *    latin1
   *    binary
   *    hex
   *    解决不支持node编码的方法是引入第三方包，npm iconv-lite
   ```js
   const fs = require('fs')
   const iconv = require('iconv-lite')

   Node 支持的编码格式
   这里乱码的原因就是：文件本身是 gbk，但是你使用 utf8 去解析，所以就失败，乱码了
   fs.readFile('./data/168305.lrc', (err, data) => {
     if (err) {
       throw err
     }
     将指定的二进制数据按照特定的编码解析成字符
     console.log(iconv.decode(data, 'gbk'))
   })
   ```
### 第三方命令行工具



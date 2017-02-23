---
title: node基础
date: 2015-8-15
categories: Node
tags: Node
---

##  node 简介
*   介绍

    -   node.js是javascript运行时（也就是解析和执行代码）的一个平台，
    -   node不是一个库、框架，也不是一门新的语言，可以说是一个解析器，可以解析和执行JavaScript，但是这里的
    javaScript指的是ECMAscript，可以执行（if ,else、for等等javaScript程序代码语句，
    所有挂在window下的方法都不属于ES，但是可以暴露接口，可以在node中使用）
    -   node是一个平台，或者可以说是一个执行环境，该环境为javascript环境提供了一些环境API
    -   javaScript语言不仅可以在浏览器环境之下运行。
    -   例如文件操作和网络操作都可以运行；
    -   node构建于 `chrome` 浏览器的 `v8 JS` 引擎以上；
        +   引擎有渲染引擎和JS解析引擎（解析和执行JS代码）
        +   Node把`chrome`浏览器的`v8 JS`解析引擎移植出来，通过基于这个引擎之上构建一些环境API，具体的环境API通过`c、c++`
        代码实现然后给上层javaScript暴露借口访问API
    -   基于node可以像其他后台语言一样，使用javaScript来进行文件操作、网络操作等等......
*   Node特性
    -   单线程（什么叫做单线程）
    -   事件驱动（如何进行事件驱动）
    -   非阻塞 I/O 模型
      + 同步阻塞IO 后续程序需要等待
      + 异步非阻塞 可以同ajax的异步一同解释 就是分模块的代码可以同时执行，互不干涉，相当于找了一个人帮你去干活儿，
      自己继续忙活自己的事情
        *   callback
        *   一般通常的异步调用API往往伴随着一个一个回调函数来接受返回值
      + 轻量和高效
      + 开源免费
      + node打破了javaScript只能在浏览器运行的局面
*   Node下载
    -   下载地址：`https://github.com/coreybutler/nvm-windows`
    -   如何验证是否有Node环境
        +   打开终端，输入 `node -v`：

            ```bash
            $ node -v
            ```
    如果能看到输出一个版本号，例如 `v7.0.0` 的文本，说明当前计算机有 Node环境。
---
##  Node操作

---
###   使用 `node` 命令执行一个 `JavaScript` 脚本文件：

   *   打开任意终端
   *   执行`cd`命令，切换到执行js脚本文件所属目录
   *   使用 `node 文件名`执行对应的js文件
   *   node会解析和执行该文件中的代码，然后将执行结果输出到终端

   ```js
   $ node main.js
   hello world

   ```

   当在终端输入  `node main.js` 命令时，实际上就是使用安装目录中的node.exe可执行程序，
   node.exe 程序会自动读取该文件中的内容，
   对`main.js`文件在node环境中解析和执行程序，如果文件中有`console.log`代码
   node会将输出在终端控制台中输出

   *    注意1：文件名不要使用 `node` 命名

   * 注意2：文件名和路径最好不要使用中文，路径中也最好不要有空格
     + 用户名不要有中文
     + 文件名也不要有中文，不要有空格，不要有乱七八糟的字符
     + 目录名也同上
     + C、D、E、F、G

   * 注意3：脚本文件编码都使用 utf8

### Node 的API


   *   需要先将模块加载到js文件中
        -   在node中本身就是一个轻量级、高度模块化平台，如果想要使用环境特定API，例如想要进行
        文件操作，那就需要加载fs核心模块（Node提供的核心模块）
        ```js
           var fs=require("fs");
        ```

        -   读取文件

        读取的文件要么是相对路径要么是绝对路径都是以 . 开头的
        ```js
        fs.readFile("./","utf-8",function(err,data){
        默认读取到的数据都是二进制的数据，可以通过toString的方法将数据进行转换将二进制数据转换为咱们可以识别的字符
        如果文件读取成功err为null；
        如果读取文件失败，err就是一个错误对象
        所以可以通过
        if(err){
            console.log(err)因为err如果是一个错误对象，终端控制台是不会抛出异常所以可以通过自定义的
            方式来监测异常
            或者是
            throw err；一般是用这种方式
        }
        来监测错误对象
        })
        ```
        ```js
        fs.readFile("./","utf-8",callback)
        第一个参数是文件路径，第二文件的编码方式，一般是设为utf-8，可以忽略
        第三个参数代码段
        ```
   -    写文件
   ```js
   var fs=require("fs")载入文件操作核心模块
   fs.writeFile("./",文本内容，callback)
   第一个参数要写入文件内容文件的路径，第二个参数是要写入的文本内容，默认编码是utf-8
   第三个参数
   function（err,data）{
   if(err){
   throw err;
   }
   console.log(data);
   }

   ```

   -    搭建http服务器
        *   加载用来构建http服务器的核心模块
        ```js
        var http=require("http");
        ```
        *   创建一个http服务器，并得到一个Server实例对象
        ```js
        var server=http.createServer();
        ```
        *   监听Server对象的request事件，设置相应的事件回调处理函数，
        请求处理函数需要传入两个参数，分别是request请求对象和response相应对象
        Request 请求对象包括：可以用来获取当前请求的一些信息，例如请求路径、请求方法、请求报文数据等
        response响应对象包括：可以用来给当前请求发送响应
        ```js
        server.on("request",function(request,response){
        write方法可以多次写入响应数据
        reponse.write("hello world")

        写入响应数据以后一定要注意结束响应，要不然客户端会认为数据没有发送完毕，还在等待接受
        可以使用 response.end 方法手动结束响应，本次请求响应结束

        response.end();
        end里可以直接闯入相应数据。
        })
        ```
           -    如果在response.write()中,写入的是中文，在访问网页的时候出现的是乱码，
           为了解决这一问题，可以用response.writeHead（）方法进行设置，
           在这个方法中可以传入三个参数；
           第一个参数：状态码
           第二个参数：状态短语，是可选参数
           第三个参数：响应头字段，是一个对象，用键值对的形式表示
           ```js
           server.on("request",function(request,response){
           response.writeHead(200,ok,{
           'Content-Type': 'text/plain; charset=utf-8'
            为了解决中文乱码的问题，可以通过在响应头中加入 Content-Type 字段来告诉客户端本次响应的数据是什么类型，以及什么编码
           })
           }
           text/plain:表示文本类型为普通文本；

           ```

           -    获取请求路径通过，request.url();
           ```js
           server.on('request', function (request, response) {
             // 可以通过请求对象拿到当前请求的一些数据，例如请求路径、方法等信息
             var url = request.url
             if (url === '/') {
               response.end('<h1>index page</h1>')
             } else if (url === '/login') {
               response.end('<h1>login page</h1>')
             } else if (url === '/about') {
               response.end('<h1>about page</h1>')
             } else {
               response.end('<h1>404 Not Found.</h1>')
             }
           })

           ```
        *   调用server的listen方法，绑定一个端口，开启服务器
        ```js
        server.listen(3000, function () {
          console.log('server is running at port 3000.')
        })
        代码发生改变时，需要重启node
        ```
### http无状态
-   客户端跟服务器交互，到底做了什么事情，对于服务器来说根本不知道

-   cookie 访问服务器
    +   客户端第一个请求过来
    +   服务器校验一下客户端有没有标记（Cookie）
    +   如果有则不发
    +   如果没有就发一个响应，并做一个标记
*   Cookie  保存状态数据

所谓的Cookie 实际上就是服务器与客户端浏览器约定好的一种规则  

*   如何使用Cookie
    -   利用响应头设置set-Cookie(放在响应报文响应头部字段中，发送给浏览器，浏览器将cookie保存起来)
    -   利用请求头获取cookie
    ```js
    res.writeHead(200,{
        "set-cookie":"isFirst=true"
    })

    ```
    cookie是以字符串的形式存储的
    document.cookie使用时需要自己通过字符串的方法自己解析
     cookie只可以存约4k的数据
     cookie会通过http请求头传递
     cookie会有域的限制
     cookie生命周期默认浏览器关闭
    
### session
*   session是基于Cookie的，Session存储就是一个对象
*   Session就是将数据存储到网站上
*   通过Cookie凭证去Session中拿数据
*   Session是一种服务器技术，一般用来保存一些安全性数据
*   session是存储在服务端的，不同的服务端语言其设置会有差异，session有多种存储方式
可以是文件形式，也可以存储到数据中，甚至内存中
*   http是一种无状态的协议，通过会话机制可以解决http无状态的问题，
cookie和session是核心
*   通过cookie存储可以在每次会话时，将用户信息传递到服务器
*   由于安全问题，通常又不会直接使用cookie，而是使用session来解决
但是单纯session不能解决无状态的，需要配合cookie实现
### cookie和Session中间件在Express中的使用
   *    安装npm install --save cookie-parser
   *    只要配置了
    



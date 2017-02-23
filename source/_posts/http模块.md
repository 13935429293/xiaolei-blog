---
title: http模块
date: 2014-8-25
tags: 前端框架
categories: 前端框架
---
### 简单实现
*   创建服务器，得到一个server对象
*   监听服务器的server对象的request请求事件，设置处理函数
    任何请求请求都会触发该request请求事件，然后执行处理函数
    也就是说所有的请求入口就是这个 request 事件
    Node 将每一个请求中的请求报文信息解析为一个对象：Request ，挂载给请求处理函数的第一个参数
    也就是说可以通过 Request 请求对象拿到一些请求报文信息，例如请求方法、请求路径、请求头部字段等信息
    同时，Node 还提供了一个接口对象：Response
    该对象可以用来给当前请求发送响应数据。
    ```js
    server.on('request', (req, res) => {
      res.end('hello world')
    })
    ```
*   绑定监听端口，启动服务器，设置启动成功之后的回调处理函数  
### 根据不同请求路径发送不同响应
*   获取当前的请求路径通过request.url
    
    这里的请求路径永远都是以 / 开头的
    例如你在浏览器地址中输入的是 http://127.0.0.1:3000  则 url 就是 /
                              http://127.0.0.1:3000/  则 url 就是 /
                              http://127.0.0.1:3000/add  则 url 就是 /add
*   请求完毕之后，可以使用 res.write 方法发送响应数据 res.write(url)  
*   注意：res.write 可以向响应流中多次发送响应数据
    但是一定要记得在写完响应流数据最后调用 res.end() 方法结束响应,否则，客户端浏览器还以为你的数据没有发送完毕，一直等待接收
*   一般发送响应数据的时候，很少有这种需要多次调用 write 方法来发送的数据
    就是说一般就是 res.write('响应数据') res.end() 结束响应
    所以，可以使用 res.end('响应数据') 直接发送响应数据，同时结束响应 
### response.end()数据处理
   +    如果你要处理一些读取出来的 html 字符串，那读文件的时候就指定编码或者调用 data.toString() 方法转为字符
   +    这里因为不处理字符串，所以就不转字符
   +    res.end() 只能接收 二进制数据或者 字符串，其它都报错
   +    如果传递的字符串，则发送响应的时候，还会自动将字符串转为二进制再发送      
###   http解析数据
   +   当客户端浏览器收到你发送的响应数据的时候
   +   浏览器会先查看响应报文头中的 Content-Type 中的 charset 编码，然后根据该编码解析数据
   +   如果响应报文头中没有 Content-Type 那么浏览器则根据 HTML 结构中的 <meta charset="UTF-8"> 来解析数据   
### 如何解决http解析数据乱码
   +   可以通过 res.writeHead 方法在结束响应之前，写响应头 ,html结构一般是按标准写法
   ```js
    res.writeHead(200, 'OK', {
            'Content-Type': 'text/html; charset=utf-8'
          })
          
   ```
### 页面中的静态资源(static)

   当浏览器获取到当前响应的 HTML 格式字符串之后
   浏览器从上到下依次解析字符串（HTML 结构文档）
   在解析的过程中，如果发现有 link img script iframe 等具有  src 或 href 的标签
   a 标签和他们不一样，a 标签是用来跳转的，资源在另一个页面
   则，浏览器主动对该资源指向的地址发起请求
   
### 对于动态资源和静态资源的一般处理方式
    静态资源可以设置路由，根据路径由外部访问，一般是包括css、js、img
    动态资源则不可以由外部访问
### http与net关系
   - net 模块中发送数据只是纯粹的发送你传入的字符串   
   - http 模块是构建与 net 模块之上的
   - http 中的收发数据还是通过 net 模块中的 Socket 收发数据的
   - http 会将收发的数据按照 HTTP 协议自动帮你解析和包装
      + 例如 http 模块自动将请求报文解析出来，然后挂载给了 req 请求对象
      + 你可以通过 req 请求对象去拿到你想要的信息 
   - 为什么既有 net 又有 http 呢？
     + http 只是一个基于 net 之上的一个模块，该模块遵循的 http 协议
     + 会对收发的数据进行 协议格式解析和包装
     + HTTP 协议只是适用于B/S模型  
   - 有的业务功能使用的是别的协议
     + 例如 一些智能终端，就用的是别的协议，而不是 HTTP
     + 但是他们都是基于最基本的 Socket 网络编程模型而构建的;   
### http协议报文解析
   
   当你在浏览器地址栏输入了一个地址：http://127.0.0.1:3000/
   浏览器按照 HTTP 协议将你输入的地址包装成 HTTP 请求报文，内容如下：
   ```js
   // GET / HTTP/1.1
   // Host: 127.0.0.1:3000
   // Connection: keep-alive
   // Cache-Control: max-age=0
   // Upgrade-Insecure-Requests: 1
   // User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/53.0.2785.116 Safari/537.36
   // Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
   // Accept-Encoding: gzip, deflate, sdch
   // Accept-Language: zh-CN,zh;q=0.8
   ```
   
   请求报文格式如下：
   
   - 请求头
     + 请求首行
       * 请求方法 请求路径 HTTP协议版本
     + 请求首部字段
       * 首都字段中放一些额外的附加信息
       * 例如 User-Agent 表示告诉服务器我这个客户端是什么
         - 这里为什么有各种浏览器的标识
         - 原因在早期的网页有各种各样兼容性问题
         - 这个字段还可以用来统计浏览器的使用量占比情况
       * Accept
         - 早期的 HTTP 0.9 中，只能收发普通字符数据 不支持图片等富文本信息
         - 历史原因，现代的服务器和客户端浏览器已经不需要这个东西
   - 请求体
       + 如果是 post 请求才有请求体
       + 如果有请求体，则请求体是在请求头的回车换行之后
       + 如果没有，也会有一个空行存在   
   
   响应报文：
   
   - 响应头
     + 响应首行
       * HTTP协议版本 状态码 状态短语
     + 响应首部字段
   - 空行
   - 响应体
     + 所有的响应数据都在响应头之后的空行之后    
### 浏览器本质

   - Socket 客户端
     - 收发数据
   - 渲染 HTML、CSS
   - 解析和执行 JavaScript 代码   
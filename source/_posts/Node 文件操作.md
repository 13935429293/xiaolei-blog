---
title: Node 文件操作
date: 2015-8-21
categories: Node
tags: Node
---

### 文件有同步调用和异步调用两种方式
   -    fs.readFileSync()//同步调用
        *   同步调用会阻塞后续代码的执行，所以效率不高
        *   同步调用需要通过try-catch来进行异常捕获
        *   同步执行文件比较符合程序员的逻辑，比较简单,就是所有任务都是自己干，而且是一步一步往下执行
   -    fs.readFile();
        *   异步API往往伴随着一个回调函数接受一个返回值或者处理异常
        *   回调函数的参数中第一个参数一般都是一个err对象用来判定API是否发生异常
        *   异步API无法通过try-catch来捕获异常，即便没有捕获异常，也不会主动抛出异常
        *   一般的文件操作，所有的异步API，都会在回调函数中提供一个err对象，
            如果操作过程有异常，err即为一个异常对象，
            如果操作过程没有异常，则err为null
        *   所以为了检测是否有异常，if(err){处理异常}；
        *   开发阶段，使用 throw err 的形式抛出异常
            目的是为了快速的定位代码的错误
            如果是网站服务器中，这个就不会去 throw err，一般会有异常处理机制
            一般在生产环境，会处理异常，例如记录日志方便排查错误
            throw err 会直接抛出异常，退出进程    
            
        
    
-   fs.writeFile(file, data, callback)：文件写入
-   fs.appendFile(file, data, callback)：文件追加
-   fs.readFile(file[, options], callback)：文件读取
-   fs.unlink(path, callback)：删除文件
-   fs.stat(path, callback)：获取文件信息
    ```js
    callback(err,stats);

    stat是一个对象,有自己的方法与属性
    stats.isFile()
    stats.isDirectory()
    stats.isBlockDevice()
    stats.isCharacterDevice()
    stats.isSymbolicLink() (only valid with fs.lstat())
    stats.isFIFO()
    stats.isSocket()
    ```
-   fs.access(path, callback)：验证文件路径是否存在
-   fs.rename(oldPath, newPath, callback)：重命名或移动文件
-   fs.watch(filename[, options][, listener]) 
    ```js
    // 文件监视 API,监视文件的变化
    // 回调函数中，需要接收两个参数
    // 第一个是当前文件的最新状态 stat
    // 第二个是变化之前的 stat
    ```
-   fs.readdir(path,callback)//读取目录
```js
    fs.readdir(path,callback);//只能读取一级目录
    callback (err,files)=>{
    err:错误对象;
    files:是一个当前目录下的所有文件名称（包括文件名和目录名）的一个数组
    
    }
```

### 文件流
 *  创建一个可读流  `readStream`
 *  创建一个可写流  `writeStream`   
 *  监听可读流对象的 data 事件    
 *  只要流对象一经创建成功，就会源源不断的使用 瓢 帮你去一瓢一瓢的读取数据
 *  当读取到一瓢数据的时候，触发 data 事件，同时将数据传递给回调处理函数
    只要读取到一点数据，就调用可写流对象的write方法，就将数据写入到可写流当中
 *  监听可读流对象的 end 事件
 *  当以流的形式读取数据结束之后，会触发可读流对象的 end 事件
    最后，使用可写流对象的end方法将可写流关闭
 *  可读流通过pipe方法，数据自动流入指定的可写流中
   readStream.pipe(writeStream);
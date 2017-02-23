---
title: promise
date: 2016-2-25
categories: Node
tags: Node
---
##  概念

    所谓Promise，就是一个对象，用来传递异步操作的消息
##  特点
   *   Promise对象代表一个异步操作，有三种状态：Pending（进行中）、
       Resolved（已完成，又称Fulfilled）和Rejected（已失败）。只有异步操作的结果，可以决定当前是哪一
       种状态，任何其他操作都无法改变这个状态。
   *   一旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise对象的状态改变，只有两种可
       能：从Pending变为Resolved和从Pending变为Rejected。只要这两种情况发生，状态就凝固了，不会再
       变了，会一直保持这个结果。就算改变已经发生了，你再对Promise对象添加回调函数，也会立即得到这
       个结果。
   *    有了Promise对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此
        外，Promise对象提供统一的接口，使得控制异步操作更加容易。
##  缺点
   *   首先，无法取消Promise，一旦新建它就会立即执行，无法中途取消。                
   *   如果不设置回调函数，Promise内部抛出的错误，不会反应到外部。
   *   当处于Pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成） 
        
        ES6规定，Promise对象是一个构造函数，用来生成Promise实例。
        
```js
  //  resolve函数的作用是，将Promise对象的状态从“未完成”变为“成功”（即从Pending变为
  //  Res），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；reject函数的作用是，
  //  将Promise对象的状态从“未完成”变为“失败”（即从Pending变为Rejected），在异步操作失败时调
  //  用，并将异步操作报出的错误，作为参数传递出去。
```        
*   promise对象容器中一般是放置一个异步任务，就是封装一个异步执行的APi
*   容器中异步任务失败之后会调用容器中的reject方法，将错误对象传递个reject
*   容器中异步任务执行成功后会调用容器中的resolve方法，将数据传递个resolve
*   当promise对象已经建立，则立即执行
*   可以通过promise容器对象的then方法节后resolve中的数据
*   then方法中需要接受一个回调函数，该回调函数其实就是resolve方法

### then
```js
p1
  .then(data => {
    console.log(data)
    return p2
  })
  .then((data) => {
    // 当前这个 then 里面的回调函数的参数，就是上一个 then 中回调函数的返回值
    // 该返回值有三种情况：
    //    1. 没有返回值，就是 undefined
    //    2. 手动的 return 普通值
    //    3. 返回一个 Promise 对象
    //        如果是 Promise 对象，那么当前这个 then 就是该 Promise 对象的 resolve 函数的结果
    console.log(data)
    throw new Error('error')
    return p3
  })
  .then(data => {
    console.log(data)
  })
  .catch(err => {
    // 当前这个 catch 方法，就可以把之前所有的任务中可能出现的异常都捕获到
    // 甚至包括 then 的回调函数中的异常也可以捕获到
    console.log(err)
  })

```
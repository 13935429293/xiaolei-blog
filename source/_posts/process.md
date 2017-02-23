---
title:  process
date: 2016-2-25
categories: Node
tags: Node
---

### process(进程对象)
   * 向标准输出写入内容（向终端写入内容）
     或者读取标准输入中的数据

   ```js
    process.stdout.write('')//输出数据到终端(相当于充当console.log的角色)
    因为在node中没有console.log
    如果想要拼接字符串,可以使用模板进行拼接
    function log(msg){
    process.stdout.write(`aaaa${msg}\n`);
    其中模板语句要写在``中,传入的变量要写在${变量}中,变量可以为表达式
    \n表示换行;
    在模板字符串中,回车换行会原样保留,类似于pre标签
    ES6语法:console.log=(msg) =>  {
    process.stdout.write(`aaaa${msg}\n`)
    }
    }
    箭头函数一般出现在匿名函数中，
   ```
   - 接受用户通过终端的输入

   ```js
    process.stdin.on("data",data=>{
    console.log(data.toString.trim())
    data.toString（）======将二进制数据转换成字符串
    data.toString（）.trim()=====用trim（）是因为终端会将enter转换为空格
    需要将空格去掉；
    })
    data 是一个二进制数据
    用户在终端输入内容，用户在敲回车之后就会触发data事件
    然后执行对应的事件处理函数，将用户输入的内容传递给回调函数
   ```
   - process.exit();退出当前进程（在终端直接输入）
   - process.faltform；运行平台
   - process.pid 获取当前运行程序的进程id
   - process.kill(进程id) 关闭一个应用程序:
   - process.arch 获取当前Node版本的位数
   - process.argv()；返回值为一个数组
        +   process.argv 可以获取当前通过执行脚本的时候传递的参数（终端传入参数）
        +   默认结果是一个数组
        +   数组中第 0 项就是 node 的可执行文件的绝对路径
        +   数组中第 1 项就是 执行的当前脚本文件的绝对路径
        +   数组中从第 2 项开始，就是用户通过执行命令传递进来的参数选项，以空格划分
   - process.end 获取环境变量
   - process.version 查看当前Node版本号
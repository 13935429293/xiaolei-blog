---
title:  使用node开发命令行复制工具
date: 2015-9-03
categories: Node
tags: Node
---
*    在入口文件的第一行起始的位置加上下面的代码 #!/usr/bin/env node 这叫设邦
*     这儿标记在 Linux 或 Unix 操作系统上就可以直接执行，./index.js 直接就可以执行
*    在 package.json 文件中加入一个 bin 字段
*     bin 字段是一个对象
*         对象中的键名用来配置生成的 *.cmd 文件的名字，也就是使用的命令名称
*                 对应的值就是输入命令之后要执行的脚本文件
*    最后，打开终端，进入当前项目的根路径，执行：npm link
*     执行完该命令之后，node 会自动帮你去全局安装路径创建对应的 cmd 文件
*    发布
   -   发布之前先去 npmjs.com 上验证一下你的 package.json 中的 name 是否被占用
   -   注意：这里一定要确保使用的镜像源地址是 npm 官方的镜像源
   -   去 npmjs.com 或者通过 npm adduser 注册一个账户
   -   npm login 登陆
   -   npm publish 发布
   -   npm version patch 更新
   -   npm unpublish 删除
*   想要在项目中使用，只需要修改package.json中的main属性，将文件指向暴露方法的那个文件
   -    现在的这个包就既可以本地安装也可以全局安装
   -    安装到本地项目就可以使用该包暴露的API接口
   -    安装到全局就可以使用该包提供的全局命令行工具
---
title: angular笔记
date: 2016-1-6
tags: 前端框架
categories: 前端框架
---

可以看Coding网站

### 什么框架和库

### 锚点

页面内部的跳转
### SPA单页应用

  - 就是利用hash替换页面的局部内容
  - 传统的网页跳转,默认向服务器发送同步请求,会发生页面跳转


### angular应用
-   引包
-   在body添加一个属性  `ng-app` ng 应用程序的入口标识
-   在body中写一个`{{'hello '+'world!'}}`
-   解析过程

    +   浏览器从上到下一次解析DOM文档
    +   当浏览器解析到body上有一个ng-app属性的时候,对于不识别的属性会xuanze忽略
    +   当浏览器解析到花括号时候,无法识别花括号里边的内容,会将他当成普通字符串进行渲染
    +   当浏览器发现script标签指定的angular.js引用文件的时候,发送请求下载该文件
    +   当angular.js程序下载成功后,自动执行程序
    +   ng会自动找到网页中具有ng-app属性的元素,然后开始解析该元素内部所有能被ng所识别的元素,也就是说ng-app在这里充当ng应用程序的入口标识,同时也是ng应用程序的管理边界。

-   当ng找到ng-app入口标识后,启动一个ng-app应用程序
-   在应用程序中开辟一个空间,这个空间其实就是一个对象
-   ng发现ng-init指令,拿到里面对应的值`name="world"`; 
-   ng自动在应用程序内部空间对象中添加一个属性`name=world`;
-   ng发现ng-model指令之后,让dom元素的例如`input`中的value和数据模型中的name建立一个绑定关系（双向绑定）
    +   当input的value值发生变化的时候,模型数据中的那么也跟着改变
    +   当模型中的name也发生改变的时候,所有绑定该属性的地方都会跟着变  
-   ng发现h1中的`{{name}}`

### 事件

### 升级版执行
-   引包
-   设置入口标识,设置控制器（让视图与$scope建立作用关系）
-   定义全局控制器函数
    +   主要目的是要将逻辑写到JavaScript代码中
    +   其次是为了拿到$Scope数据模型对象
    +   通过操作$scope数据模型和视图做交互
-   根据视图暴露模型数据成员
    + 给$scope初始化一些数据成员
    +  同时暴露一些行为函数
-   angular闪烁解决方式
    解决方式有两种:
    +  将ng脚本引入到head中,ng官方推荐将ng脚本在最上边引入
    +  在所有使用了表达式的外部节点上加一个属性`ng-cloak`,当加上`ng-cloak`的时候,那个不会等待DOM onload执行结束就会先在加了那个-cloak的
        
    +  ng在启动执行的时候会自动向head中插入一个style样式
    +  当ng解析完毕之后,ng-cloak会自动移除
    -  在所有使用表达式的地方都通过ng-bind指令来代替
        +   也就是说使用了ng-bind完全可以替代表达式
        +   用了他可以解决就算脚本在底部引入也不闪烁的问题
-   ng默认默认是在document.onload 的时候开始解析执行
-   所有的$scope都有一个根作用域$rootscope

###   如何划分控制器

   - 一个页面中按照不同的功能划分不同的功能模块
   -    定义一个模块
   
   ```js
   // 注意：必须指定第二个参数,否则变成获取已定义的模块
   var demoApp = angular.module('DemoApp', [])(创建一个模块,没有任何依赖项)
   ```
   
    
### 模块加载
   
   -    创建了一个模块（没有任何依赖项）
   -   Angular 中的模块不像一些 CMD、CommonJS、AMD 等模块定义规范
         +  有输入有输出
         +  Angular 中的模块中的输入与输出（加载模块依赖、暴露模块成员）体现不够明显
   -   Angular 中的模块也不能通过代码去主动的加载另一个模块
   -   Angular 中的模块唯一体现的地方在于：
         +  解决原来全局控制器命名污染的问题
         +  解决按照不同的页面,将不同的控制器组织到一起
         ```js
           angular.module('Demo1App', [])
           .controller('Demo1Controller', ['$scope', function ($scope) {
             $scope.name = 'Demo1Controller'
           }])
     
         angular.module('Demo2App', [])
           .controller('Demo2Controller', ['$scope', function ($scope) {
             $scope.name = 'Demo2Controller'
           }])
         // 手动引导模块的启动,可以启动多个模块,但是都必须作用到一个元素上
         // 这种方式不推荐使用
         angular.bootstrap(document.getElementById('body'), ['Demo1App', 'Demo2App'])       
         
         ```
       

### 处理html标签
   * 如果想要绑定 HTML 字符串
   * 则必须使用这种方式
      - npm install --save angular-sanitize
      - 在使用该字符串的所属的模块中加载 ngSanitize 模块即可生效
      ```js
       angular.module('DemoApp', ['ngSanitize'])
       .controller('DemoController', ['$scope', function ($scope) {
       $scope.title = '母猪的生产过程'
       $scope.content = '<script>window.alert("hello")<\/script><p>作者：xxx</p> <p><strong>母猪</strong>的生产过程</p>'
               }])
      ```
      
### `ng-repeat`遇到的坑
* `ng-repeat` 在遍历普通数据类型的时候,如果有相同的值,会报错
### 在异步请求当中修改了数据模型为什么不起作用？
   +    如果在普通的定时器函数内部修改了 $scope 视图模型成员
   +    一定要通过 $scope.$apply() 手动刷新视图模型才行
   
   ```js
   angular.module('DemoApp', [])
         .controller('DemoController', ['$scope', '$timeout', function ($scope, $timeout) {
           $scope.loading = true
           setTimeout(function () {
               $scope.loading = false
               
           * $scope.$apply() * 
              
             }, 10000)
             // $timeout(function () {
             //   $scope.loading = false
             // }, 2000)
         }])
   ```
### ng-hide-show-if-switch 区别
面试题：ng-if-hide-show 的区别
*   ng-if 是直接就不渲染这个 DOM 了

  当为 true 的时候直接渲染
  当为 false 的时候直接移除该元素
*   ng-hide
*   ng-show

    两者无论是 true 还是 false 元素都在,是通过样式来控制的

### ng-src

所有需要动态指定 src 的地方都通过 ng-src 来替换,否则浏览器会真的对这个 src 发起请求
### 单项数据的绑定

  ng 中也提供了一些单向数据绑定的指令
  也就是说只能通过模型获取到数据
  但是不能通过改变元素值而影响视图模型数据
### ng中的过滤器
*  date 
```
{{time: date:'yyyy-mm-dd HH:mm:ss'}}
```


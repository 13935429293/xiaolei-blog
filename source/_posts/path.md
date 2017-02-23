---
title:  path模块（momentjs.）
date: 2016-2-25
categories: Node
tags: Node
---

-   path.basename(path[, ext]): 获取文件名部分
    
    获取最后一个路径借口
    ```js
    path.basename('/foo/bar/baz/asdf/quux.html')
      // returns 'quux.html'
    
    path.basename('/foo/bar/baz/asdf/quux.html', '.html')
      // returns 'quux'
    
    ```

-   path.dirname(path): 获取目录部分
    
    返回一个路径的目录
    ```js
    path.dirname('/foo/bar/baz/asdf/quux')
    // returns '/foo/bar/baz/asdf'
    ```
-   path.extname(path): 获取扩展名部分
    ```js
    path.extname('index.html')
    // returns '.html'

    path.extname('index')
    // returns ''
    
    path.extname('.index')
    // returns ''
    ```

-   path.isAbsolute(path): 判断是否是绝对路径

    ```js
    path.isAbsolute('/baz/..')  // true
    path.isAbsolute('qux/')     // false
    path.isAbsolute('.')
    ```
-   path.join([...paths]): 将多个路径拼接为一个路径
```js
    path.isAbsolute('/baz/..')  // true
    path.isAbsolute('qux/')     // false
    path.isAbsolute('.')

```
-   path.normalize(path): 将一个非标准路径转为一个标准路径
```js
    path.normalize('/foo/bar//baz/asdf/quux/..')
    // returns '/foo/bar/baz/asdf'
    //返回值为/..之前路径的目录

```
-   path.parse(path):将路径转换为路径对象
    ```js
    path.parse('/home/user/dir/file.txt')
    // returns
    // {
    //    root : "/",
    //    dir : "/home/user/dir",
    //    base : "file.txt",
    //    ext : ".txt",
    //    name : "file"
    // }
    ```
-   path.resolve([...paths]): 将多个路径拼接为一个绝对路径
```js
    path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif')
    // if the current working directory is /home/myself/node,
    // this returns '/home/myself/node/wwwroot/static_files/gif/image.gif'
```

-   path.sep: 获取操作系统路径分隔符"\,\\,/"

```js
    'foo/bar/baz'.split(path.sep)
    // returns ['foo', 'bar', 'baz']
```

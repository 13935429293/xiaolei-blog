---
title: gulp
date: 2016-1-10
tags: 前端框架
categories: 前端框架
---

*   首先在顶级目录下有一个
*   加载gulp
    +   transport = require('gulp-seajs-transport');
    +   
*   gulp.task(任务名称，function(){
    gulp.src('./public/!(libs)/**/*.js')
    .pipe(transport())
    .pipe(gulp.dest(存在哪个目录))
    
})



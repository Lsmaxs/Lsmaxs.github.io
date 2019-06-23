---
title: 全栈成长之core
date: 2019-06-20 18:02:42
tags: stack
---

# 控制台
在`Node.js`中，使用`console`对象代表控制台(在操作系统中表现为一个操作系统指定的字符界面，比如 `Window`中的命令提示窗口)。

- console.log
- console.info
- console.error 重定向到文件
- console.warn
- console.dir
- console.time
- console.timeEnd
- console.trace
- console.assert

# 全局作用域

- 全局作用域(global)可以定义一些不需要通过任何模块的加载即可使用的变量、函数或类
- 定义全局变量时变量会成为global的属性
- 永远不要不使用var关键字定义的变量，以免污染全局作用域 (会挂载在global)
- setTimeout clearTimeout
- setInterval clearInterval
- unref和ref

```
let test = function(){
  console.log('callback');
}
let timer = setInterval(test,1000);
timer.unref();
setTimeout(function(){
  timer.ref();
},3000)

```

# 函数

- require
- 模块加载过程
- require.resolve
- 模板缓存(require.cache)
- require.main
- 模块导出
```
module.exports, require, module, filename, dirname
```

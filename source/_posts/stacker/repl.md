---
title: 全栈成长之REPL
date: 2019-06-18 19:29:42
tags: stack
---

# REPL  

在`Node.js`中为了使开发者方便测试`JavaScript`代码，提供了一个名为REPL的可交互式运行环境。开发者可以在该运行环境中输入任何`JavaScript`表达式，当用户按下回车键后，REPL运行环境将显示该表达式的运行结果。

## 如何进入REPL   
在命令行容器中输入`node`命令并按下回车键，即可进入REPL运行环境。

## REPL操作  
- 变量的操作，声明普通变量和对象
- eval
- 函数的书写
- 下划线访问最近使用的表达式
- 多行书写
- REPL运行环境中的上下文对象  
```
let repl = require('repl');
let con = repl.start().context;
con.msg = 'hello';
con.hello = function(){
    console.log(con.msg);
}
```

## REPL运行环境的基础命令  
- .break 退出当前命令
- .clear 清除REPL运行环境上下文对象中保存的所以变量与函数
- .exit 退出REPL运行的环境
- .save 把输入的所有表达式保存到一个文件中
- .load 把所有的表达式加载到REPL运行环境中
- .help 查看帮助命令
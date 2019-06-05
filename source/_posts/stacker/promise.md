---
title: 全栈成长之Promise
date: 2019-06-02 09:19:42
tags: stack
---
# 异步回调 
### 回调地狱
在需要多个操作的时候，会导致多个回调函数嵌套，导致代码不够直观，就是常说的回调地狱

### 并行结果
如果几个异步操作之间并没有前后顺序之分,但需要等多个异步操作都完成后才能执行后续的任务，无法实现并行节约时间 

# Promise
`Promise`本意是承诺，在程序中的意思就是承诺我过一段时间后会给你一个结果。 什么时候会用到过一段时间？答案是异步操作，异步是指可能比较长时间才有结果的才做，例如网络请求、读取本地文件等

# promise的三种状态  
- pending Promise对象实例创建时候的初始状态
- Fulfilled 可以理解为成功的状态
- Rejected 可以理解为失败的状态
> `then` 方法就是用来指定`Promise `对象的状态改变时确定执行的操作，`resolve` 时执行第一个函数（onFulfilled），`reject` 时执行第二个函数（onRejected）

# 构造一个Promise
使用 Promise
```
let promise = new Promise((resolve, reject) => {
    setTimeout(() => {
        if(Math.random()>0.5)
            resolve('This is resolve!');
        else
            reject('This is reject!');
    }, 1000);
});
promise.then(Fulfilled,Rejected)
``` 
- 构造一个Promise实例需要给promise构造函数传入一个函数 
- 传入的函数需要有两个形参,两个形参都是function类型的参数 
  - 第一个形参运行后会让promise实例处于resolve状态,所以我们一般给第一个形参命名为resolve,使Promise对象的状态改变成功,同时传递一个参数用于后续成功后的操作
  - 第一形参运行后会让Promise实例处于reject状态,所以我们一般给一个形参命名为reject,将Promise对象的转态变为失败,同时将错误的信息传递到后续错误处理的操作
---
title: 全栈成长之Async
date: 2019-05-21 15:26:39
tags: stack
---

# JavaScript 异步

## 异步

- __'异步'__-简单来说就是吧任务分为两段，先执行第一段先前段，然后转而执行其他任务，等准备好了，再回过头执行第一段后半段的回调。  
- 比如：执行文件读取任务，异步如下：  
 ![ASYNC1](../../images/stacker/asyncfunc1.png)

- 这种不连续的执行，就叫做 __异步__。相应的，连续的执行，叫做 __同步__  
 ![ASYNC2](../../images/stacker/syncfunc2.png)


## 高阶函数
  
函数作为一等公民，可以作为参数和返回值。

### 可以用于批量生成函数  
```
let toString = Object.prototype.toString;
let isString = obj => toString.call(obj) == `[object String]`
let isArray = obj => toString.call(obj) == `[object Array]`
let isType = type => obj => Object.prototype.toString.call(obj) == `[object ${type}]`;


```

### 可以用于需要调用多次才执行的函数
```
let after = (time,task)=> () => {
    if(times--==1){
        return task.apply(this,arguments)
    }
}
let fn = after(4,()=>console.log('hello guys'));
fn();

```

## 异步语法目标

> 异步编程的语法目标，就是怎样让它更像同步编程  

- 回调函数实现
- 事件监听
- 发布订阅
- Promise / Genterator
- Asycn / await  

## 回调  

所谓的回调函数，就是把任务第二段需要执行的函数单独写到一个函数中，等到回头执行这个任务时，就直接调用这个函数

```
fs.readFile( '某个文件', (err,data)=>{
    if(err) throw err;
    console.log(data);
})

```  
这是一个错误优先处理的回调函数(ERROR-FIRST CALLBACKS),这个也是`Node.js`本身的特色之一。

## 回调的问题  
### 异常处理  
```


```


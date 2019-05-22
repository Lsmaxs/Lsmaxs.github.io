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
- Promise / Generator
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
try{
    // xxx
} catch(err){
    // TODO
}
```  
异步代码时 `try catch` 不在生效  
```
let async = callBack => {
    try{
        setTimeout(()=>{ callBack();},1000)
    } catch (err) {
        console.log('捕获错误', e)
    }
}

async(()=>{console.log(t)});
```
因为这个回调函数被封存了起来，直到下一个事件环的时候才会取出，`try`只能捕获当前循环内的异常，对于callBack异步无能为力。  

`Node`在处理异常有一个约定，将异常作为回调的第一实参传回，如果为空表示没有出错。

```
async ((err,callBack)=>{
    if(err){
        console.log(err);
    }
})

```  
异步方法也要遵循两个原则  
- 必须在异步之后调用传入的回调函数
- 如果出错了要向回调函数传入异常供调用者判断
```
let async = callBack => {
    try{
        setTimeout(()=>{
            if(success){
                callBack(null)
            }else{
                callBack('错误');
            }
        })
    } catch(err){
        console.log('捕获错误',e)
    }
}

```

### 回调地狱
异步多级依赖的情况下会导致嵌套非常深，代码难以阅读和维护

```
let fs = require('fs');
fs.readFile('template.txt','utf8',(err,template)=>{
    fs.readFile('data.txt','urf8',(err,data)=>{
        console.log(`${template}:${data}`)
    })
})

```


## 异步流程解决方案

### 事件发布 / 订阅模式  
订阅事件实现了一个事件和多个回调函数的关联
```
let fs = require('fs);
let EventEmitter = require('events);
let eve = new EventEmiter();
let html = {};
eve.on('ready',(key,value)=>{
    html[key] = value;
    if( Object.keys(html).length ==2 ) {
        console.log(html);
    }
})
function render(){
    fs.readFile('template.txt','utf8',(err,tempalte)=>{
        eve.emit('ready','template',tempalte)
    })
    fs.readFile('data.txt','utf8',(err,data)=>{
        eve.emit('ready','data',data)
    })
}
render();
```

### 哨兵变量

```
let fs = require('fs');
let after = (times,callBack)=>{
    let result = {};
    return (key,value) => {
        result[key] = value;
        if(Object.keys(result).length == times){
            callBack(result);
        }
    }
}
let done = after(2,(res)=>{
    console.log(res)
})
function render(){
  fs.readFile('template.txt','utf8',function(err,template){
    eve.emit('ready','template',template);
  })
  fs.readFile('data.txt','utf8',function(err,data){
    eve.emit('ready','data',data);
  })
}
render();
```  

### Promise/Deferred模式  

### 生成器 Generator / yield  
- 当你在执行一个函数的时候，你可以在某个节点暂停函数的执行，并且做一些其他工作，然后再返回这个函数继续执行，甚至是携带一些新的值，然后继续执行。  
- 上面描述的场景正是`javaScript`生成器函数所致力于解决的问题。当我们调用一个生产器函数的时候，它并不会立即执行，而是需要我们手动的去执行迭代操作（`next`方法）。也就是说，你调用生成器函数，他会返回给你一个迭代器。迭代器会遍历每个中断点。
- `next`方法返回值的`value`属性，是`Generator` 函数向外输出数据；`next`方法还可以接受参数，这是向`Generator`函数体内输入数据

### 生成器的使用

```
function *foo(){
    let index = 0;
    while(index<10>){
        yield index++; // 暂停函数的执行，并执行yield后的操作
    }
}
var bar = foo(); // 返回的其实是一个迭代器

console.log(bar.next()) // { value: 1,done: false }
console.log(bar.next()) // { value: 2,done: false }
console.log(bar.next()) // { value: 10,done: true }
```

### Co
`co`是一个为 `Nodejs` 和浏览器打造的基于生成器的流程控制工具，借助于`Promise`，你可以使用更加优雅的方式编写非阻塞代码。
```
let fs = require('fs');
function readFile(filename){
    return new Promise( (resolve,reject)=>{
        fs.readFile(filename,(err,data)=>{
            if(err) {
                return reject(err)
            }
            resolve(data)
        })
    })
}
function *read(){
    let template = yield readFile('./template.txt')
    let data = yield readFile('./data.txt')
    return template + data
}
co(read).then(data=>{
    console.log(data)
},err=>{
    console.log(err)
})

```

```
function co (gen){
    let it = gen();
    return new Promise((resolve,reject)=>{
        !function next(lastValue){
            let {value,done} = it.next(lastValue);
            if(done){
                resolve(value)
            }else{
                value.then(next,reason=>reject(reason))
            }
        }()
    })
}
```

## Async / await  
使用`async`关键字，你可以轻松地达到`Generator`和`co`达到的效果

### Async的优点  
- 内置执行器
- 更好地语义  
- 更广的适用性  
```
let fs = require('fs');
function readFile(filename){
    return new Promise((resolve,reject)=>{
        fs.readFile(filename,'utf8',(err,data)=>{
            if(err){
                reject(err)
            }else{
                resolve(data)
            }
        })
    })
}

async function read(){
    let template = await readFile('./tempalte.txt')
    let data = await readFile('./data.txt')
    return tempalte + data
}
let resultData = read();
resultData.then(data=>console.log(data))
```
### async函数的实现  
`async`函数的实现，就是将`Generator`和自动执行器，包装在一个函数里

```
async function read() {
  let template = await readFile('./template.txt');
  let data = await readFile('./data.txt');
  return template + '+' + data;
}

// 等同于
function read(){
  return co(function*() {
    let template = yield readFile('./template.txt');
    let data = yield readFile('./data.txt');
    return template + '+' + data;
  });
}

```

#### [Async](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/async_function) 、[Generator](http://www.ruanyifeng.com/blog/2015/04/generator.html)
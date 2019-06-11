---
title: 全栈成长之Promise
date: 2019-06-08 10:19:42
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
1. 使用 Promise
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

2. es5模拟Promise
```
function Promise(fn){
    fn((data)=>{
        this.success(data)
    },(error)=>{
        this.error()
    })
}
Promise.prototype.resolve = function(data){
    this.success(data);
}
Promise.prototype.reject = function(error){
    this.error(error)
}
Promise.prototype.then = function (success, error) {
    this.success = success;
    this.error = error;
}
```

3. es6模拟Promise
```
class Promise {
    constructor(fn){
        fn((data)=>{
            this.success(data)
        },(error)=>{
            this.error()
        })
    }
    
    resolve(data){
        this.success(data);
    }
    
    reject(error){
        this.error(error)
    }

    then(success,error){
        this.success = success
        this.error = error;
        console.log(this)
    }
}

```

# Promise做为函数的返回值
```
function ajaxPromise(queryUrl){
    return new Promise((resolve,reject)=>{
        let xhr = new XMLHttpRequest();
        xhr.open("GET",quertUrl,true);
        xhr.send(null);
        xhr.onreadystatechange = () => {
            if(xhr.readyState==4){
                IF(xhr.status===200){
                    resolve(xhr.responseText);
                }else{
                    reject(xhr.responseText);
                }
            }
        }
    })
}

ajaxPromise('http://www.baidu.com')
  .then((value) => {
    console.log(value);
  })
  .catch((err) => {
    console.error(err);
  });
```

# promise的链式调用
- 每次调用返回的都是一个新的Promise实例
- 链式调用的参数通过返回值传递

then可以使用链式调用的写法原因在于，每一次执行该方法时总是会返回一个`Promise`对象
```
readFile('1.txt').then(function (data) {
    console.log(data);
    return data;
}).then(function (data) {
    console.log(data);
    return readFile(data);
}).then(function (data) {
    console.log(data);
}).catch(function(err){
 console.log(err);
});

```
# Promise API
1. Promise.all
- 参数： 接受一个数组，数组内都是`Promise`实例
- 返回值： 返回一个`Promise`实例，这个`Promise`实例的状态转移取决于参数的`Promise`实例的状态变化。当参数中所有的实例都处于`resolve`状态时,返回的`Promise`实例会变成`resolve`状态。如果参数中任意一个实例处于`reject`状态,返回的Promise实例变为`reject`状态。
```
Promise.all([p1, p2]).then(function (result) {
  console.log(result); // [ '2.txt', '2' ]
});
```
> 不管两个promise谁先完成，Promise.all方法会按照数组里面的顺序将结果返回

2. Promise.race
- 参数： 接受一个数组，数组内都是Promise实例
- 返回值： 返回一个`Promise`实例，这个`Promise`实例的状态转移取决于参数的`Promise`实例的状态变化。当参数中任何一个实例处于`resolve`状态时，返回的`Promise`实例会变为`resolve`状态。如果参数中任意一个实例处于`reject`状态，返回的`promise`实例变为`reject`状态。
```
Promise.race([p1, p2]).then(function (result) {
  console.log(result); // [ '2.txt', '2' ]
});
```

3. Promise.resolve
返回一个`Promise`实例，这个实例处于`resolve`状态。
根据传入的参数不同有不同的功能：
- 值(对象，数组，字符串等): 作为`resolve`传递出去的值
- `promise`实例：原封不动返回

4. Promise.rejcet
返回一个`Promise`实例，这个实例处于`reject`状态。
- 参数一般就是抛出的错误信息

# co 
## co初体验
```
let fs = require('fs');
function getNumber(){
    return new Promise((resolve,reject)=>{
        setTimeout(function(){
            let number = Math.random();
            number>.5?resolve(number):reject('数字太小')；
        },500)
    })
}

function *read(){
    let a = yield getNumber();
    console.log(a);
    let b = yield 'b';
    console.log(b);
    let c = yield getNumber();
    console.log(c);
}

function co(gen){
    return new Promise((resolve,reject)=>{
        let g = gen();
        function next(lastValue){
            let { done,value} = gen.next(lastValue);
            if(done){
                resolve(lastValue)
            }else{
                if(value instanceof Promise){
                    value.then(next,function(val){
                        reject(value)
                    })
                }else{
                    next(value)
                }
            }
        }
    })
}

co(read).then(res=>console.log(res),rea=>console.log(rea));

```
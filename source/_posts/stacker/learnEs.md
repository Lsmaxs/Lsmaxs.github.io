---
title: 全栈成长之ES6
date: 2019-05-28 09:19:33
tags: stack
---

# ECMAScript6

ECMAScript简称就是ES,你可以把它看成是一套标准,`JavaScript`就是实施了这套标准的一门语言,现在主流浏览器使用的是ECMAScript5。

[ECMAScript6](https://es6.ruanyifeng.com/) 全部看完受用一生～   

## 作用域变量  
作用域就是一个变量的作用范围。也就是你声明一个变量以后,这个变量可以在什么场合下使用 以前的`JavaScript`只有全局作用域，还有一个函数作用域。

### var的问题
1. var 没有块级作用域，定义后在当前闭包中都可以访问，如果变量名重复，就会覆盖前面定义的变量，并且也有可能被他人修改。
```
if(true){
    var a = 'a';
}
console.log(a);
```
2. var在for循环标记变量共享，一般在循环中使用的会被共享，其本质上也是由于没有块级作用域造成的。
```
for(var i=0;i<4;i++){
    setTimeout(function(){
        alert(i)
    },0)
}

```
> 结果  弹窗四次 4

### 块级作用域
在用var定义变量的时候，变量是通过闭包进行隔离的，现在用了let，不仅仅可以通过闭包隔离，还增加了一些块级作用域隔离。块级作用域用一组大括号定义一个快，使用let定义的变量在大括号的外面是访问不到的。
1. 实现块级作用域
```
if(true){
    let a = 'test';
}
console.log(a); // ReferenceError: a is not defined

if(true){
    let name = 'Sean';
}
console.log(window.name);  // undefined
```
2. for 循环中也可以使用i
```
// 嵌套循环不会相互影响
    for(let i = 0;i<3;i++){
        console.log('out',i);
        for(let i = 0;i<2;i++){
            console.log('in',i)
        }
    }
```
>  结果 out 0 in 0 in 1 out 1 in 0 in 1 out 2 in 0 in 1  
3. 重复定义会报错  
```
if(true){
    let a = 2;
    let a = 4; // Identifier 'a' has already been declared
}
```
4. 不存在变量的预解释
```
for (let j = 0; i < 3; i ++){
    console.log('inner',i);
    let i = 100;
}
```
> 结果 i is not defined
6.  闭包新写法 
```
以前 
(function(){
    // TODO
})();
现在
{
    // TODO
}
```
## 常量  
使用const我们可以去声明一个常量，常量一旦赋值就不能再修改了
1. 常量不能重新赋值
```
const MY_NAME = 'Sean';
MY_NAME = 'zfpx2';//Assignment to constant variable
```
2. 常量值可改变
> 注意 const限制的是不能给变量重新赋值，而变量的值本身是可以改变的,下面的操作是可以的
```
const names = ['zfpx1'];
names.push('zfpx2');
console.log(names);
```
3. 不同的块级作用域可以多次定义  
```
const A = "0";
{
    const A = "A";
    console.log(A)
}
{
    const A = "B";
    console.log(A)
}
console.log(A)
```
> 结果 A B 0  

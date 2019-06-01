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

## 解构
1. 解构数组  
解构意思就是分解一个东西的解构，可以用一种类似数组的方式定义N个变量，可以将一个数组中的值按照规则赋值过去。
```
let [ name,age ] = [ 'Sean',18 ];
console.log(name,age)
```
2. 嵌套赋值
```
let [x,[y],z] = [1,[1,2]];
console.log(x,y,z) // 1  [1,2] undefined

let [x,[y,z]] = [1,[1,2]];
console.log(x,y,z) // 1 1 2

let [json,arr,num] = [{name:'Sean'},[1,2],3];
console.log(json,arr,num); // {name:'Sean'} [1,2] 3
```
3. 省略赋值  
```
let [ , , x] = [1,2,3]
console.log(x) // 3
```

4. 解构对象
```
var obj = {name:'Sean',age:18};
//对象里的name属性的值会交给name这个变量，age的值会交给age这个变量
var {name,age} = obj;
//对象里的name属性的值会交给myname这个变量，age的值会交给myage这个变量
let {name: myname, age: myage} = obj;
console.log(name,age,myname,myage); // Sean 18 Sean 18
```
5. 默认值  
在赋值和传参的时候可以使用默认值  
```
let [a='a',b='b',c= new Error('c必须指定')] = 【1，，3】
console.log(a,b,c);

function ajax(options){
    var method = options.method || "get";
    var data = options.data || {};
    ...
}

function ajax({method = "get", data}){
    console.log(arguments);
}
ajax({
    method: "post",
    data: {"name": "zfpx"}
});
```
## 字符串  
1. 模板字符串  
模板字符串用反引号（数字1左边的那个键）包含，其中的变量用`${}`括起来。
```
var name = 'Sean',age = 18;
let desc = `${name} is ${age} old`
console.log(desc);

//所有模板字符串的空格和换行，都是被保留的
let str = `<ul>
<li>a</li>
<li>b</li>
</ul>`;
console.log(str);
```
> 其中的变量会用变量值替换掉
```
function replace(desc){
    return desc.replace(/\$\{([^{]+)\}/g,function(matched,key){
        return eval(key);
    });
}
```

2. 带标签的模板字符串 
可以在模板字符串的前面添加一个标签，这个标签可以去处理模板字符串 标签其实就是一个函数，函数可以接受两个参数，一个是`string`，就是模板字符串里的每个部分的字符串，还有一个参数可以使用`rest`的形式`values`，这个参数里面是模板字符串里的值
```
let name = 'Sean',age = 18;
function desc(strings,...values){
    console.log(strings,values);
}
desc`${name} is ${age} old!`;

```
3. 字符串新方法  
- include()： 返回布尔值，表示找到了参数字符串。
- startsWith()： 返回布尔值，表示参数字符串是否在源字符串的头部
- endsWith()：返回布尔值，表示参数字符串是否在源字符串的尾部
```
var s = 'Sean';
s.startsWith('S') // true
s.endsWith('n') // true
s.includes('a') // true
```
第二个参数，表示开始搜索的位置
```
var s = 'Sean';
console.log(s.startsWith('e',2)); // true
console.log(s.endsWith('e',2)); // true
console.log(s.includes('S',2)); // false

```
> `endsWith`的行为与其他两个方法有所不同。它针对前n个字符，而其他两个方法针对从第n个位置直到字符串结束

4. repeat 
`repeat`方法返回一个新字符串，表示将原字符串重复N次  
```
'x'.repeat(3); // xxx
'x'.repeat(0); // ''
```

## 函数  
1. 默认参数  
可以给定义的函数接受的参数设置默认的值 在执行这个函数的时候，如果不指定函数的参数的值，就会使用参数的这些默认的值。 
```
function ajax(url,method='GET',dataType="json"){
  console.log(url);
  console.log(method);
  console.log(dataType);
}
```

2. 展开操作符  
把...放在数组前面可以把一个数组进行展开，可以把一个数组直接传入一个函数而不需要使用`apply`  
```
// 传入参数
let print = function (a,b,c){
    console.log(a,b,c)
}
print([1,2,3]);
print(...[1,2,3]);

// 可以代替apply
let m1 = Math.max.apply(null,[8, 9, 4, 1]);
let m2 = Math.max(...[8, 9, 4, 1]);

// 可以代替concat
let arr1 = [1,3];
let arr2 = [3,5];
var arr3 = arr1.concat(arr2);
var arr4 = [...arr1, ...arr2];
console.log(arr3,arr4);

//类数组的转数组
function max(a,b,c) {
    console.log(Math.max(...arguments));
}
max(1, 3, 4);
```
3. 剩余操作符 
剩余操作符可以把其余的参数的值都放到一个叫`b`的数组里面 
```
let rest = function(a,...rest){
    console.log(a,rest)
}
rest(1,2,3); // 1, [2,3]
```
4. 解构参数  
```
let destruct = function({name,age}){
    console.log(name,age);
}
destruct({name:'zfpx',age:6});
```
5. 函数的名字 
ECMAScript 6 给函数添加了一个`name`属性 
```
var desc = function descname(){}
console.log(desc.name);
```
6. 箭头函数 
箭头函数简化了函数的定义方法，一般以`=>`操作符左边为输入的参数，而右边则是进行的操作以及返回的值`inputs=>output`  
```
[1,3,6].forEach(val=>val*6)
```
> 输入参数如果多于一个要用括号包裹，函数体如果需要执行多语句也需要用大括号包裹   
箭头函数根本没有自己的`this`，导致内部的`this`都是外层外码块的`this`。正是因为它没有`this`，从而避免了`this`指向的问题。
```
var person = {
    name:'zfpx',
    getName:function(){
-   setTimeout(function(){console.log(this);},1000); //在浏览器执行的话this指向window
+   setTimeout(() => console.log(this),1000);//在浏览器执行的话this指向person
    }
}
person.getName();

```
7. 数组的新方法

- from  
将一个数组或者类数组变成数组，会复制一份  
```
let newArray = Array.from(oldArray)
```
- Array.of
`of`是为了将一组数值，转换为数组
```
console.log(Array(3), Array(3).length);
console.log(Array.of(3), Array.of(3).length);
```
- cpoyWithin  
`Array.prototype.copyWithin(target,start=0,end = this.length)`覆盖目标的下标 开始的下标 结束的后一个下标
```
[1, 2, 3, 4, 5].copyWithin(3, 1, 2); //[1, 2, 3, 2, 5]
```
- find  
查到对应的元素和索引  
```
let arr = [1, 2 ,3, 3, 4, 5];
let finds = arr.find((item, index, arr) => {
    return item === 3;
});
let findIndexs = arr.findIndex((item, index, arr) => {
    return item === 3;
});

console.log(finds, findIndexs);// 3  2
```
- fill  
就是填充数组的意思 会更改原数组 `Array.prototype.fill(value, start, end = this.length)`
```
 let arr = [1, 2, 3, 4, 5, 6];
 arr.fill('a', 1, 2);
 console.log(arr) //  [1, "a", 3, 4, 5, 6]
```

## 对象 
1. 对象字面量  
如果你想在对象里面添加跟变量一样的属性，并且属性的值就是变量表示的值就可以直接在对象里加上这些属性  
```
let name = 'Sean';
let age = 18;
let getName = function(){
    console.log(this.name);
}
let person = {
    name,
    age,
    getName
}
person.getName();
```
2. Object.is 
对比两个值是否相等
```
console.log(Object.is(NaN,NaN));
```
3. Object.assign
把多个对象的属性复制到一个对象中，第一个参数是复制的对象，从第二个参数开始往后，都是复制的源对象
```
let nameObj = {name:'Sean'};
let ageObj = {age:18};
let Obj = {address:'dxc'};
Object.assign(Obj,nameObj,ageObj);

//克隆对象
function clone (obj) {
  return Object.assign({}, obj);
}
```
4. Object.setPrototypeOf
将一个指定的对象的原型设置为另一个对象或者null
```
let nameObj1 = {name:'Sean1'};
let nameObj2 = {name:'Sean2'};
let obj = {};

Object.setPrototypeOf(obj,nameObj1);
console.log(obj.name); //Sean1
console.log(Object.getPrototypeOf(obj)); //{name: "Sean1"}
Object.setPrototypeOf(obj,nameObj2);
console.log(obj.name); //Sean2
console.log(Object.getPrototypeOf(obj)); //{name: "Sean2"}

```
5. proto
直接在对象表达式中设置prototype
```
let obj1  = {name:'Sean'};
let obj3 = {
    __proto__:obj1
}
console.log(obj3.name); // Sean
console.log(Object.getPrototypeOf(obj3)); //{name: "Sean"}
```
6. super
通过`super`可以调用`prototype`上的属性和方法
```
let person ={
    eat(){
        return 'milk';
    }
}
let student = {
    __proto__:person,
    eat(){
        return super.eat()+' bread'
    }
}
console.log(student.eat());
```

## 类 
1. class  
使用`class`这个关键词定义一个类，基于这个类创建实例以后会自动执行`constructor`方法，此方法可以用来初始化  
```
class Person {
    constructor(name){
        this.name = name;
    }
    getName(){
        console.log(this.name);
    }
}
let person = new Person('Sean');
person.getName();

```
2. get与set
`getter`可以用来得获取属性，`setter`可以去设置属性  
```
class Person{
    constructor(){
        this.hobbies = [];
    }
    set hobby(hobby){
        this.hobbies.push(hobby);
    }
    get hobby(){
        return this.hobbies
    }
}
let person = new Person();
person.hobby = 'basketball';
person.hobby = 'football';
console.log(person.hobby); //["basketball", "football"]
```
3. 静态方法 static  
在类里面添加静态的方法可以使用`static`这个关键词，静态方法就是不需要实例化类就能用的方法
```
class Person {
   static add(a,b){
       return a+b;
   }
}
console.log(Person.add(1,2));
```
4. 继承 extends
一个类可以去继承其他的类的东西
```
class Person {
   constructor(name){
     this.name = name;
   }
}
class Teacher extends Person{
    constructor(name,age){
        super(name);
        this.age = age;
    }
}
var teacher = new Teacher('Sean',18);
console.log(teacher.name,teacher.age);
```

##  生成器（Generator）与 迭代器（Iteration）
`Generator`是一个特殊的函数，他执行会返回一个 `Iterator`对象。通过遍历迭代器，`Genrator`函数运行后会返回一个遍历器对象，而不是返回普通函数的返回值。
1. Iteration模拟
迭代器有一个`next`方法，每次执行的时候回返回一个对象，对象里面有两个属性，一个是`value`表示返回的值，还有就是布尔值`done`表示迭代是否完成
```
funciton bug(books){
    let i = 0;
    return {
        next(){
            let done = i == books.length;
            let value = !done?  books[i++]:undefined;
            return{
                value,
                done
            }
        }
    }
}
let iterators = buy(['js', 'html']);
var curr;
do {
    curr = iterators.next();
    console.log(curr);
} while (!curr.done);
```
2. Generator  
生成器用于创建迭代器  
```
function *buy(books){
    for(var i=0;i<books.length;i++){
        yield hooks[i]
    }
}
let buying = buy(['js','html']);
var curr;
do {
    curr = buying.next();
    console.log(curr);
} while (!curr.done);
```

## 集合  
1. Set  
一个`Set`是一堆东西的集合，`Set`有点像数组，不过跟数组不一样的是，`Set`里面不能有重复内容  
```
let books = new Set();
books.add('js');
books.add('js');//添加重复元素集合的元素个数不会改变
books.add('html');
books.forEach(function(book){//循环集合
    console.log(book);
})
console.log(books.size);//集合中元数的个数
console.log(books.has('js'));//判断集合中是否有此元素
books.delete('js');//从集合中删除此元素
console.log(books.size);
console.log(books.has('js'));
books.clear();//清空 set
console.log(books.size);
```

2. Map  
可以使用 `Map` 来组织这种名值对的数据  
```
let books = new Map();
books.set('js',{name:'js'});//向map中添加元素
books.set('html',{name:'html'});
console.log(books.size);//查看集合中的元素
console.log(books.get('js'));//通过key获取值
books.delete('js');//执照key删除元素
console.log(books.has('js'));//判断map中有没有key
books.forEach((value, key) => { //forEach可以迭代map
    console.log( key + ' = ' + value);
});
books.clear();//清空map
```

## 模块 
可以根据应用吧需求代码分成
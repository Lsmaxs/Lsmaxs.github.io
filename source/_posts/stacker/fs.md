---
title: 全栈成长之fs
date: 2019-07-12 16:20:12
tags: stack
---

# fs模块
- 在node.js中，使用fs模块来实现所有有关文件以及目录的创建、写入以及删除操作
- 在fs模块中，所有的方法都分为同步和异步两部实现
- 具有sync后缀的方法为同步方法，不具有sync后缀的方法为异步方法

# 整体读取文件
## 异步读取
```
fs.readFile(path[,options],callback)
```
- options
  - encodeing
  - flag flag 默认 = 'r'

## 同步读取
```
fs.readFileSync(path[,options])
```

# 写入文件
## 异步写入
```
fs.writeFile(file,data[,options],callback)
```
- options
 - encoding
 - flag flag默认 = 'w'
 - mode 读写权限，默认为0666

```
let fs = require('fs');
fs.writeFile('./1.txt',Data.now()+'\n',{flag:'a'},function(){
    console.log('OK')
})
```

### 同步写入
```
fs.writefileSync(file,data[,options])
```

### 追加文件
> fs.appendFile(file,data[,options],callback)
```
fs.appendFile('./1.txt',Date.now()+'\n',function(){
    console.log('ok')
})
```
### 拷贝文件
```
function copy(){
    fs.readFile(src,function(err,data){
        fs.writeFile(target,data)
    })
}
```

## 从指定位置处开始读取文件

### 打开文件
> fs.open(filename,flags,[mode],callback);
- FileDescriptor 是文件描述符
- FileDescriptor 可以被用来表示文件
- in -- 标准输入(键盘)的描述符
- out -- 标准输出(屏幕)的描述符
- err -- 标准错误输出(屏幕)的描述符  

```
fs.open('./1,txt','r',0600,function(err,fd){});
```

### 读取文件
> fs.read(fd,buffer,offset,length,postion.callback((err,bytesRead,buffer)))
```
const fs = require('fs');
const path = require('path');
fs.open(path.join(__dirname,'1.txt'),'r',0o666,function(err,fd){
    console.log(err);
    let buf = Buffer.alloc(6);
    fs.read(fd,buf,0,6,3,function(err,bytesRead,buffer){
        console.log(bytesRead);
       console.log(buffer===buf);
       console.log(buf.toString());
    })
})
```

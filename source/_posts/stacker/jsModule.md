---
title: 全栈成长之JsModule
date: 2019-05-22 16:20:12
tags: stack
---


# JS模块化方面的不足  
- JS没有模块系统，不支持封闭的作用域和依赖管理
- 没有标准库，没有文件系统和IO流API
- 也没有包管理系统

# CommonJS规范
- 封装功能
- 封闭作用域
- 可能解决依赖问题
- 工作效率更高，重构方便

# Node中的CommonJS
- 在node.js 里，模块划分所有的功能，每个JS都是一个模块
```
(function(exports,require,module,__filename,__dirname){
  exports = module.exports={}
  exports.name = 'zfpx';
  exports = {name:'zfpx'};
  return module.exports;
})

```

# 模块分类
## 原生模块  
`http` `path` `fs` `util` `events` 编译成二进制,加载速度最快，原来模块通过名称来加载

## 文件模式  
在硬盘的某个位置，加载速度非常慢，文件模块通过名称或路径来加载  
文件模块的后缀有三种
- 后缀名为.js的JavaScript脚本文件,需要先读入内存再运行
- 后缀名为.json的JSON文件,fs 读入内存 转化成JSON对象
- 后缀名为.node的经过编译后的二进制C/C++扩展模块文件，可以直接使用  
> 一般自己写的通过路径来加载,别人写的通过名称去当前目录或全局的node_modules下面去找

## 第三方模块 
- 如果require函数只指定名称则视为从`node_modules`下面加载文件，这样的话你可以移动模块而不需要修改引用的模块路径
- 第三方模块的查询路径包括`module.paths`和全局目录

### 全局目录
window如果在环境变量中设置了`NODE_PATH`变量，并将变量设置为一个有效的磁盘目录，require在本地找不到此模块时向在此目录下找这个模块。 UNIX操作系统中会从 `$HOME/.node_modules $HOME/.node_libraries`目录下寻找
---
title: 全栈成长之Module
date: 2019-05-25 13:29:12
tags: stack
---

# 模块化

模块化是指把一个复杂的系统分解到多个模块以方便编码。

## 命名空间 
开发网页要通过命名空间的方式来组织代码  
```
<script src='jquery.js'></script>
```
- 命名空间冲刺，两个库可能会使用同一个名称  
- 无法合理地管理项目依赖和版本  
- 无法方便地控制依赖的加载顺序  

## CommonJs  
`CommonJs`是一种使用广泛的`JavaScript`模块化规范，核心思想是通过`require`方式来同步地加载依赖的其他模块，通过``module.exports`导出需要暴露的接口。  

### 用法
采用`CommonJs`导入以及导出的代码如下：
```
// 导入
const antFun  = require('./moduleA_antFun');
antFun();
// 导出
module.exports = antFun;
```  

### 原理
A.js
```
let fs = require('fs);
let path = require('path');
let b = req('./b.js');
function req(mod){
    let fileName = path.join(__dirname,mod);
    let content = fs.readFileSync(fileName,'utf8');
    let fn = new Function('exports', 'require', 'module', '__filename', '__dirname', content + '\n return module.exports;');
    let module = {
        exports: {}
    };
    return fn(module.exports,req,module,__fileName,__dirname)
}

```  
B.js
```
console.log('I am B');
exports.name = 'zfpx';
```

## AMD
AMD 也是一种 `JavaScript`模块化的规范，与 `CommonJS`最大的不同在于他采用异步的方式去加载依赖模块。 `AMD` 规范主要是为了解决针对浏览器环境的模块化问题，最具代表性的实现是`requirejs`。

AMD的优点  
- 可在不转换代码的情况下直接在浏览器中运行
- 可加载多个依赖
- 代码可运行在浏览器环境和`Node`环境下  

AMD的缺点  
- `JavaScript`运行环境没有原生支持AMD，需要先导入了AMD的库后才能正常使用。  

### 用法  
```
// 定义一个模块
define('a',[],function(){
    return 'a'
})  
define('b',['a'],function(a){
    return a + 'b'
})

// 导入和使用 
require(['b'],function(b){
    console.log(b)
})
```
### 原理
```
let factorise = {};
funtion define(modName, dependencies,factory){
    factory.dependencies = dependencies;
    factorise[modName] = factory
}
function require(modNames,callBack){
    let loadeModNames = modNames.map(function(modName){
        let factory  = factorise[modName];
        let dependencies = factory.dependencies;
        let exports;
        require(dependencies,function(...mods){
            exports = factory.apply(null,mods);
        });
        return exports
    })
    callBack.apply(null,loadeModNames)
}
```

## ES6模块化
ES6模块化是ECMA提出的`JavaScript`模块化规范。他在语言层面上实现了模块化。浏览器厂商和`Node.js` 都宣布要原生支持该规范。它将逐步取代`CommonJs`和AMD规范，成为浏览器和服务器统一的模块解决方案。采用ES6模块化导入以及导出的代码如下
```
// 导入
import {name} from './person.js';
// 导出
export const name = 'zfpx';
```
ES6 模块虽然是终极模块化方案，但他的缺点在于目前还无法直接运行在大部分`JavaScript`运行环境下，必须通过工具转换成标准ES5后才能正常运行。

# 自动化构建
构建就是把源代码转换成发布到线上的可执行`JavaScript`、`CSS`、`HTML`代码，包括以下内容。 
- 代码转换： ECMAscript6 编程成浏览器识别的 ECMAscript5、LESS编译成CSS等。
- 文件优化： 压缩`JavaScript`、`CSS`、`HTML`代码，压缩合并图片等。
- 代码分割： 提取多个页面的公共代码，提取首屏不需要执行部分的代码让其异步加载
- 模块合并： 在采用模块化的项目里会有多个模块和文件，需要构建功能吧模块分类合并成一个文件。
- 自动刷新： 监听本地源代码的变化，自动重新构建，刷新浏览器。
- 代码校验： 在代码被提交到仓库前需要校验代码是否符合规范，以及单元测试是否通过。
- 自动发布： 更新完代码后，自动构建出线上可发布代码并推送至发布系统。

```
#! /usr/bin/env node
const Path = require('path');
const fs = require('fs');
let ejs = require('ejs');
let cwd = process.cwd();
let { entry,output:{filname,path}} = reuqire(Path.join(cwd,'./webpack.config.js'));
let script  = fs.readFileSync(entry,'utf8');
script.replace(/require\{["'](.+?)['"]\}/g,function(){
    let name  = arguments[1];
    let script = fs.readFileSync(name,'utf8');
    modules.push({name,script});
})
let bundle = `
(function (modules) {
    function require(moduleId) {
        var module = {
            exports: {}
        };
        modules[moduleId].call(module.exports, module, module.exports, require);
        return module.exports;
    }
    return require("<%-entry%>");
})
    ({
        "<%-entry%>":
            (function (module, exports, require) {
                eval(\`<%-script%>\`);
            })
       <%if(modules.length>0){%>,<%}%>
        <%for(let i=0;i<modules.length;i++){
            let module = modules[i];%>   
            "<%-module.name%>":
            (function (module, exports, require) {
                eval(\`<%-module.script%>\`);
            })
        <% }%>    
    });
`
let bundleJs = ejs.render(bundle,{
    entry,script,modules
})

try{
    fs.writeFileSync(Path.join(path,filename),bundlejs);
}catch(e){
     console.error('编译失败 ', e);
}
console.log('compile sucessfully!');
```
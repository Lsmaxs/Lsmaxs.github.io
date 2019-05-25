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


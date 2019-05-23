---
title: 全栈成长之Command
date: 2019-05-22 16:20:12
tags: stack
---

# MAC

## 查看端口占用

### lsof
```
lsof -i:prot

lsof -i:8080
```

### netstat
```
netstat -antp | grep port

netstat -antp | grep 8080

```

## 进程

### 杀死进程  
查看进程占用
```
lsof -i tcp:8080
```  
该命令会显示占用8080端口的进程，有其PID，可以通过PID关掉该进程  
```
kill PID
```  

---
title: 全栈成长之buffer
date: 2019-07-12 16:20:12
tags: stack
---

# 什么是Buffer
- 缓冲区Buffer是暂时存放输入输出数据的一段内存。
- JS语言没有二进制数据类型，而在处理TCP和文件流的时候，必须要处理二进制数据。
- NodeJS提供了一个Buffer对象来提供对二进制数据的操作。
- 是一个表示固定内存分配的全集对象，也就是说要放到缓存区中的字节数需要提前确定。
- Buffer好比由一个8位字节元素组成的数组，可以有效的在JavaScript中表示二进制数据

# 什么是字节
- 字节(Byte)是计算机存储时的一种计量单位，一个字节等于8位二进制数
- 一个位代表一个0或1，每8个位组成一个字节
- 字节是通过网络传输信息的基本单位
- 一个字节最大值十进制是255(2**8-1)

# 进制
- 0b 2进制
- 0x 16进制
- 0o 8进制

## 转为十进制
将任意进制字符串转换为十进制
```
parseInt('11', 2) // 3
parseInt('77', 8) // 63
parseInt('E1', 16) //231
```
## 将十进制转换为其他进制字符串
```
(3).toString(2)
(63).toString(8)
(231).toString(16)
```

# 定义buffer的三种方式

## 通过长度定义buffer
```
// 创建一个长度为 10 、 且用0 填充的buffer
const buf1 = Buffer.alloc(10);
// 创建一个长度为 10 、 且用0 填充的buffer
const buf1 = Buffer.alloc(10,1);
// 创建一个长度为 10 、 且未初始化的buffer
const buf3 = Buffer.allocUnsafe(10)
```

## 通过数组定义buffer
```
// 创建一个包含 [0x1, 0x2, 0x3] 的 Buffer。
const buf4 = Buffer.from([1, 2, 3]);
```
> 正常情况下为0-255之间

## 字符串创建
```
const buf5 = Buffer.from('Sean 博客')
```

# buffer的常用方法

## fill
`buf.fill(value[,offset[,end]][,encoding])`
```
 buffer.fill(0);
```

## write方法
`buf.write(string[, offset[, length]][, encoding])`
```
let buffer = Buffer.allocUnsafe(6);
buffer.writer('Sean',0.3,'utf8')
buffer.writer('Blog',4.3,'utf8')
```

## writeInt8 
- 通过指定的`offset`将`value`写入到当前Buffer
- 这个value应当是一个有效的有符号的8位整数
`buf.writeInt8(value, offset[, noAssert])`

```
let buf = Buffer.alloc(4);
buf.writeInt8()
buf.writeInt8(0,0);
buf.writeInt8(16,1);
buf.writeInt8(32,2);
buf.writeInt8(48,3);
console.log(buf);// <Buffer 00 10 20 30>
console.log(buf.readInt8(0));//0
console.log(buf.readInt8(1));//16
console.log(buf.readInt8(2));//32
console.log(buf.readInt8(3));//48
```

## Little-Endian & Big-Endian
不同的CPU有不同的字节序类型，这些字节序是指整数在内存中报错的顺序。
- Big-endian: 将高序字节存储在起始地址(高位编址)
- Little-Endian: 将低序字节存储在起始地址(低位编址)
```
let buffer = Buffer.alloc(4);
buffer.writeInt16BE(2**8,0)
buffer.writeInt16LE(2**8,0)

```
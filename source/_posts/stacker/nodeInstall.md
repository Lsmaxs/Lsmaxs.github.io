---
title: 全栈成长之NodeInstall
date: 2019-06-18 02:19:42
tags: stack
---

# Node.js 安装配置

## MAC安装

### 安装包安装
下载Mac安装后结束后，单击下载的文件，运行它，会出现一个向导对话框。 单击continue按钮开始安装，紧接着向导会向你询问系统用户密码，输入密码后就开始安装。不一会儿就会看见一个提示Node已经被安装到计算机上的确认窗口

### homebrew安装

1. 先安装homebrew打开网站 [http://brew.sh/](http://brew.sh/);
2. 在terminal下安装Homebrew
```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

homebrew依赖ruby,如果安装出错检查一下ruby的版本以及路径

```
ruby -v
```

3. 通过homebrew安装node.js
```
brew install node
```

4. 其它软件也都可以通过homebrew安装
```
brew install mongodb redis git
```

### n模块安装
切换版本或升级node可以安装 n 模块
```
npm install n -g
```  
> 直接输入 n 即可上下切换不同的版本  


## 源代码安装

### 安装依赖库
Node依赖一些第三方代码库，但幸运的是大多数第三方库已经跟随Node一起发行，如果想从源码编译，需要以下两项工具
- Python（2.4及以上版本）
- libssl-dev 如果计划使用SSL/TLS加密，则必须安装它。`libssl`是openssl工具中用到的库，在linux和UNIX系统中，通常可以用你喜欢的包管理器安装`libssl`,而在Mac OS X系统中已经预置了。

### 下载源代码

选择好版本后，你就可以到nodejs.org网站上复制对应的tar压缩包进行下载，比如你用的mac或linux,可以输入以下命令下载
```
wget https://nodejs.org/dist/v8.9.4/node-v8.9.4.tar.gz


curl -O https://nodejs.org/dist/v8.9.4/node-v8.9.4.tar.gz
```
如果这二种工具都没有可以下载这二个工具或者从网站上点击链接下载

### 编译源码
对tar压缩包进行解压

- x extract 解包
- f file 要解包的是个文件
- z gzip 这个包是压缩过的，需要解压缩
- v verbose把解包过程告诉你
```
tar xfz node-v8.9.4.tar.gz
```
进入源代码目录
```
cd node-v8.9.4
```
对其进行配置
```
./configure
```
现在就开始编译了
```
make
```
编译生成Node可执行文件后，就可以按以下的指令开始安装
```
make install
```

以上指令会将Node可执行文件复制到`/user/local/bin/node`目录下

执行以上指令如果遇到权限问题，可以以root用户权限执行

```
sudo make install
```
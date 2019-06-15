---
title: 全栈成长之stack
date: 2019-06-08 10:19:42
tags: stack
---


# 栈
```
class Stack{
    constrance(object){
        this._list = [];
    }

    push(item){
        this._list.push(item);
    }

    pop(){
        return this._list.pop();
    }

    peek(){
        return this.isEmpty()?this._list[this._list.length]: null
    }

    isEmpty(){
        return this._list.length > 0
    }

    size(){
        return this._list.length
    }
}

```

# 队列

```
 class Queue {
    constrance(object){
        this._list = [];
    }

    peek(item){
        this._list.push(item);
    }

    pop(){
        return this._list.unshift();
    }

    size(){
        return this._list.length
    }

    isEmpty(){
        return this.size() > 0;
    }
 }


```

# 双端队列

```
class DeQueue{
    constrance(object){
        this._list = [];
    }

    add_Fpeek(item){
        this._list.insert(0,item);
    }

    add_Lpeek(item){
        this,_list.push(item)
    }

    Fpop(){
        return this._list.unshift();
    }
    
    Lpop(){
        return this._list.pop();
    }

    size(){
        return this._list.length
    }

    isEmpty(){
        return this.size() > 0;
    }


}



```
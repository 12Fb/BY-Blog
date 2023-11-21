---
title: 创建原型链的相关api
---
本类型文章主要用于节省时间,快速的查到并了解最重要的特性
### Object.create(obj,...args)
创建一个对象,并将将新对象的隐式原型指向```obj```


### Object.setPrototype(obj,prototype)
用于设置一个对象的**显式原型**

#### 自己实现一个new函数
首先理一下new函数做的事
1. 创建一个空对象,设置隐式原型
2. 设置显式原型
3. 调用显式原型中的构造函数并同时设置this指向新对象 -- constructor
4. 返回该对象
下面来看一下代码:
```js
//实现一个new操作符
let proto = {
    logger() {
        console.log('father');
    }
}
let obj = {
    a: 1, b: 2,
    init(x, y) {
        this.a = x;
        this.b = y;
    }
}

function con(obj, ...args) {
    const temp = Object.create(obj)
    Object.setPrototypeOf(temp, obj)
    const init = obj.init;
    init.apply(temp, args);
    return temp;
}
const temp = con(obj, 4, 5);
console.log(temp);
```
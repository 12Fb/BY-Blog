---
title: 装饰器
hide_title: true
key_word: ['装饰器','设计模式',Decorator]
---


### 函数装饰器
函数装饰器其实就是高阶函数,直接上代码
```js
//装饰器
function decorator(fun) {
    return function(...args){
        //也可以装饰一下result,看具体需求
        const result = fun(...args)
        console.log('装饰器');
        return result;
    }
}
//原函数
function sum(x,y) {
    return x + y;
}
//使用装饰器 
const newSum = decorator(sum);
newSum(1,2);
```


#### 类装饰器
在2023年 11.18,```javascript```暂时还不支持类装饰器的语法, typescript支持

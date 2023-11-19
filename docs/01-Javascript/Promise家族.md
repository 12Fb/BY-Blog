---
title: Promise 家族
---

### Promise

Promise 对象有三种状态：

1. Pending（进行中）： 当创建一个 Promise 对象时，它的初始状态是 pending（进行中）。这意味着 Promise 的异步操作还没有完成，也没有成功解决或拒绝。

2. Fulfilled（已解决）： 当 Promise 的异步操作成功完成并返回一个值时，Promise 进入 fulfilled（已解决）状态。在这个状态下，Promise 的解决值可用，并可以通过 then 方法的回调函数进行处理。

3. Rejected（已拒绝）： 当 Promise 的异步操作失败、出现异常或被拒绝时，Promise 进入 rejected（已拒绝）状态。在这个状态下，Promise 可能携带一个拒绝原因（通常是一个错误对象），并可以通过 catch 或 then 方法的第二个参数的回调函数进行处理。

#### Promise.all() -- es2015

接收一个  数组,数组中的所有**Promise都```fulfilled```了或者有任何的``reject``**之后返回一个结果数据,按原数组中``promise对象`的顺序摆放

```javascript
Promise.all([p1, p2, p3]).then((value) => {
	console.log(value);
});
```

#### Promise.allSettled() -- es2020

接收一个  数组,数组中的所有**Promise 都解析好了(包括```reject```和```resolve```)**返回一个按原数组顺序的值数组,里面会包括解析的状态

```javascript
Promise.allSettled([p1, p2, p3]).then((value) => {
	console.log(value);
});
```

#### Promise.any() -- es2020

接收一个  数组,返回**第一个 resolve 的值**

```javascript
Promise.any([p1, p2, p3]).then((value) => {
	console.log(value);
});
```

#### Promise.race() -- es2018

接收一个  数组,返回**第一个解析的值**,如果是`reject`的值要用`catch`捕获
**注意**: 跟其他Promise方法不同,race()是异步的,若数组为空则永远也不会缓存

```javascript
Promise.race([p1, p2, p3]).then((value) => {
	console.log(value);
});
```


#### 第一层深入,promise方法实现
不难发现,上面的promise方法中,没有一个方法可以获取最后一个promise,既然如此我们不如自己来写一个```Promise_Last(array:any[])```来获取最后一个resolve的Promise的对象

```javascript title="Promise_Last"
function Promise_Last(arr = []) {
    let index = 0;
    let count = arr.length;
    let result = [];
    return new Promise((resolve,reject)=>{
        for(let i = 0 ; i< arr.length ;i++){
            Promise.resolve(arr[i])
            .then(res =>{
                //这里决定着数组中的内容是否按顺序摆放
                result.push(res)
                index++;
            console.log(res,index,result);
            if(index === count){
                //这里可以决定着返回最后一个,还是整个数组
                resolve(result[count-1]);
            }
            })
            .catch(err=>
            reject(err))
        }
    })
}
```
接下来可以用Promise.then()去接收这个值,看看是不是返回最后一个被resolve的值,在代码中的注释部分,稍微进行修改就可以实现Promise.all(),Promise.rase()等



#### 第二层深入, promise本身是如何实现调度的
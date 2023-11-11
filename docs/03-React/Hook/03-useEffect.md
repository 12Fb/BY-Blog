---

---

```useEffect``` 是一个 React Hook，它允许你 将组件与外部系统同步。


### 更新State
- 在setState中提到过state要看作可读，如果实在想要修改，用```setState(n => n+1)```形式,所以在useEffect中使用这种方式**更新依赖**


### 依赖项
- useEffect可以指定依赖项来进行更新, ```useEffect(setup,dependencies)```, 这也是他用来连接外部系统的原因之一
没有依赖项,也有几种选择:<br/>
```useEffect(setup) //只要重新渲染就执行```<br/>
```useEffect(setup,[]) //只在初始渲染执行```

#### 带来的一些问题
- 当一个State**同时是依赖项并且又在useEffect中被改变时,显然会出现死循环的问题**,所以一定要避免
- useEffect允许一个返回值作为cleanup函数,在组件被卸载的时候会执行这个函数,确保你对一些副作用进行了卸载,不然他们可能会造成严重的性能浪费
---
title: useMemo
---

`useMemo` 是一个 React Hook，它在每次重新渲染的时候能够缓存计算的结果。
`jsx const cachedValue = useMemo(calculateValue, dependencies)`

### 参数(官方)

- calculateValue：要缓存计算值的函数。它应该是一个没有任何参数的**纯函数**，并且可以返回任意类型。React 将会在首次渲染时调用该函数；在之后的渲染中，如果 dependencies 没有发生变化，React 将直接返回相同值。否则，将会再次调用 calculateValue 并返回最新结果，然后缓存该结果以便下次重复使用。

- dependencies：所有在 calculateValue 函数中使用的响应式变量组成的数组。响应式变量包括 **props、state 和所有你直接在组件中定义的变量和函数**。如果你在代码检查工具中 配置了 React，它将会确保每一个响应式数据都被正确地定义为依赖项。依赖项数组的长度必须是固定的并且必须写成 [dep1, dep2, dep3] 这种形式。React 使用 Object.is 将每个依赖项与其之前的值进行比较。

### 最佳实践

#### 什么时候需要 useMemo

如果某个交互因为重复渲染超过**1ms**,那`useMemo`就是有意义的,可以简单的用一些 api 测试时间:

```jsx
console.time("start");
//...业务代码
console.log("end");
```

### 用法

#### 跳过重复渲染

来看一下 memo 失效的例子,用 useMemo 修复

```jsx
const List = memo(function List({ items,theme }) {
  // ...
});
export function TodoList({todos}){
    //每次渲染都是一个新的数组,尽管值相同
    const todo = filterTodos(todos)
    return (
        { /* 如果上面的theme变了,List也会重新渲染, memo就失效了 */}
        <div theme={theme}>
        <List item={todo} ></List>
        </div>
    )
}
```

这时候就需要对 filterTodos 进行 memo 化

```jsx
const List = memo(function List({ items,theme }) {
  // ...
});
export function TodoList({todos}){
    //每次渲染都是一个新的数组,尽管值相同
    const todo = useMemo(()=>{
        filter(todos)
    },[todos])
    return (
        { /* 如果上面的theme变了,List也会重新渲染, memo就失效了 */}
        <div theme={theme}>
        <List item={todo} ></List>
        </div>
    )
}
```

#### 记忆一个函数,为什么会有 useCallback

先看一下没有 useMemo 的代码,Form 将不会被记忆

```jsx
const Page =() =>{
    function Submit(form){
        ...
    }
    return(
        {/* 这里传递了一个函数,每次渲染都会创建一个新的Sumbit */}
        <Form onSubmit={Submit} />
    )
}
```

使用 useMemo

```jsx
const Page =() =>{
   const Submit = useMomo(()=>{
    return (form)=>{
        oldSubmit(form)
    }
   },[form])
    return(
        {/* Sumbit被缓存了 */}
        <Form onSubmit={Submit} />
    )
}
```

尽管这样是有效的,但是看起来很傻,因此有了`useCallback`,唯一用处就是避免这种嵌套

```jsx title="使用useCallback"
...
const Submit = useCallback(form=>{
    oldSubmit(form);
},[form])
...
```

可以大概这么理解

```jsx
function useCallback(fn, deps) {
	useMemo(() => fn, deps);
}
```

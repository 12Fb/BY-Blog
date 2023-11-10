## useState

### 官方的摘要

- 当一个组件需要在多次渲染间“记住”某些信息时使用 state 变量。
- State 变量是通过调用 useState Hook 来声明的。
- Hook 是以 use 开头的特殊函数。它们能让你 “hook” 到像 state 这样的 React 特性中。
- Hook 可能会让你想起 import：它们需要在非条件语句中调用。调用 Hook 时，包括 useState，仅在组件或另一个 Hook 的顶层被调用才有效。
- useState Hook 返回一对值：当前 state 和更新它的函数。
- 你可以拥有多个 state 变量。在内部，React 按顺序匹配它们。
- State 是组件私有的。如果你在两个地方渲染它，则每个副本都有独属于自己的 state。

### 最佳实践

**因为 setState 总是传入一个新的值,所以对数组和对象的更新有一些注意的地方**

#### 更新对象

```javascript title="对象更新"
const obj = {
	a: 1,
	b: "string",
};
const [Obj, setObj] = useState(obj);
// 用setState更新 a 属性为2
setObj({
	...obj,
	a: 2,
});
```

#### 更新数组

```javascript title="数组更新 => 不要直接修改当前数组"
const arr = [1, 2, 3];
const [Arr, setArr] = useState(arr);
// 添加元素: concat [...arr]展开语法
setArr([...arr, 4]);
setArr(arr.concat([4]));

//删除元素: filter, slice
arr.filter((t) => t !== 4);
arr.slice(arr.indexOf(4));

//替换元素: map
arr.map((v) => (v === 4 ? 5 : v)); //4替换成5

//排序: 先将数组copy一下
const tempArr = [...arr];
tempArr.sort();
setState(tempArr);

//总之就是要传入一个新的值
```

### 深入 useState

#### React 渲染机制

来看一段代码

```jsx title="count.js"
const Button  = () => {
    const [count, setCount] = useState(0);
    const change = () =>{
     setCount (count+1);
     setCount (count+1);
     setCount (count+1);
 }
 ...
}
```

这段代码中虽然执行了 santState 三次，但是渲染结果却只渲染了一次，这是为什么呢？

- 首先来看一下渲染流程: **setState 触发渲染 -> react 对比当前快照更新组件-> 提交到浏览器更新 dom**
  其中这个**快照** 是是否要更新的关键,每次渲染都会找当前状态的快照,如果快照没有变化,则不更新组件,如果快照有变化,则更新组件,问题是快照在生成的时候 State 并没有变化,所以参考的快照中 count 为 0
- 改变当前快照,快照中的状态值也是可以改变的
  我们将上面的代码改一改:
  ```jsx title="count.js"
    const Button  = () => {
    const [count, setCount] = useState(0);
    const change = () =>{
     setCount (n=>n+1);
     setCount (n=>n+1);
     setCount (n=>n+1);
    }
    ...
    }
  ```

#### 总结

- 通常来说,State 不会再一次渲染中有所改变(useEffect 可以打破这个规则)
- 快照: 惰性生成,两种快照的对比可以让 react 知道哪些东西要更新
- 渲染: 当前所有**将要执行的**setState 都找到只会,react 才会将他们执行,这也是三个 setCount(count+1)并没有更改当前 state,的原因,每一次都是跟 count:0 这个快照对比,自然三次的结果都会让 count=1
- 强制更新当前快照里面的状态: setState(n=>n+1)

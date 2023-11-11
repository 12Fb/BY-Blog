---
title: useReducer
---

`useRedcer`允许你处理组件中的复杂逻辑,就好像 Flux 架构中 dispatcher

### 创建一个 Reducer

```javascript
const [state,dispatch]  = useReducer(reducer,initArg,init?)
```

- **Reducer**: 用来更新状态的**纯函数**
  reducer 中可能会有很多个 action
- **dispatch**: 触发 reducer 中的 action,`dispatch({type: 'add'})`,必须指定一个 action type
  来看一个例子

```javascript
const [state, dispatch] = useReducer(reducer, { age: 123, name: "456" });
dispatch({ type: "add" });
diapatch({
	type: "rename",
	name: "hby",
});
function reducer(state, action) {
	switch (action.type) {
		case "add": {
			return {
				age: state.age + 1,
			};
		}
		case "rename": {
			return {
				name: action.newName,
			};
		}
	}
}
```

- **initArg**: 用于初始化 state 的值
- **init**: 可选参数, 会覆盖 initArg,初始值-> init(initArg)

### 避免重新创建初始值

```javascript
function TodoList({ username }) {
	const [state, dispatch] = useReducer(reducer, createInitialState(username));
}
```

createInitialState 每次渲染都会被调用,但是其初始值缺只被用于初始化 state,所有改正一下写法

```javascript
function TodoList({ username }) {
	const [state, dispatch] = useReducer(reducer, usename, createInitialState);
}
```

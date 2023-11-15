---
title: Profiler
---
```<Profiler>``` 允许你编程式测量 React 树的渲染性能,生产环境默认禁用。
```jsx
<Profiler id="App" onRender={onRender}>
  <App />
</Profiler>
```
在```onRender```中可以接收各项数值
```jsx
function onRender(id, phase, actualDuration, baseDuration, startTime, commitTime) {
  // 对渲染时间进行汇总或记录...
}
```
#### 参数
- id：字符串，为 ```<Profiler>``` 树的 id 属性，用于标识刚刚提交的部分。如果使用多个 profiler，可以通过此属性识别提交的是树中的哪一部分。
- phase：为 "mount"、"update" 或 "nested-update" 中之一。这可以让你知道组件树是首次挂载还是由于 props、state 或 hook 的更改而重新渲染。
- actualDuration：在此次更新中，渲染 ```<Profiler>``` 组件树的毫秒数。这可以显示子树在使用记忆化（例如 memo 和 useMemo）后的效果如何。理想情况下，此值在挂载后应显著减少，因为许多后代组件只会在特定的 props 变化时重新渲染。
- baseDuration：估算在没有任何优化的情况下重新渲染整棵 ```<Profiler>``` 子树所需的毫秒数。它通过累加树中每个组件的最近一次渲染持续时间来计算。此值估计了渲染的最差情况成本（例如初始挂载或没有使用记忆化的树）。将其与 actualDuration 进行比较，以确定记忆化是否起作用。
- startTime：当 React 开始渲染此次更新时的时间戳。
- commitTime：当 React 提交此次更新时的时间戳。此值在提交的所有 profiler 中共享，如果需要，可以对它们进行分组。
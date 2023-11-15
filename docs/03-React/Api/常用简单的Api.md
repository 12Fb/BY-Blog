---
  title: 简单Api
  date: 2023-11-14
  description: 由于比较简单,这里只是简单描述他的作用
---

### forwardRef

`forwardRef` 允许你的组件使用 ref 将一个 DOM 节点暴露给父组件, 该 ref 可以逐级暴露给嵌套在更深层的组件

```jsx
import { forwardRef } from "react";

const Son = forwardRef((props, ref) => {
	// ..
});
```

### lazy

`lazy` 能够让你在组件第一次被渲染之前延迟加载组件的代码, 配合 Suspense 在加载的时候显示加载态

```jsx
import { lazy } from "react";

const MarkdownPreview = lazy(() => import("./MarkdownPreview.js"));
```

**不要滥用 refs。只有对于作为 props 无法表达的 命令式 行为才应该使用 ref：例如滚动到节点、将焦点放在节点上、触发动画，以及选择文本等等**<br/>
**更通俗点说就是: 上层组件要控制下层组件 dom**

### memo

`memo` 允许你的组件在 props 没有改变的情况下跳过重新渲染。

```jsx
const MemoizedComponent = memo(SomeComponent, arePropsEqual?)
```

**`memo`允许在第二个参数中传入一个比较函数来自定义比较**

### startTransition

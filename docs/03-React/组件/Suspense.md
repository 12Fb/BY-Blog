
```<Suspense>``` 允许在子组件完成加载前展示后备方案。
```jsx
<Suspense fallback={<Loading />}>
  <SomeComponent />
</Suspense>
```
- 用法
  - 当内容正在加载时显示后备方案
  - 同时展示内容
  - 逐步加载内容
  - 在新内容加载时展示过时内容
  - 阻止隐藏已经显示的内容
  - 表明 transition 正在发生
  - 在导航时重置 Suspense 边界
  - 为服务器错误和客户端内容提供后备方案

### 逐步加载内容
可以嵌套着使用达到逐步加载的效果
```jsx
<Suspense fallback={<BigSpinner />}>
  <Biography />
  <Suspense fallback={<AlbumsGlimmer />}>
    <Panel>
      <Albums />
    </Panel>
  </Suspense>
</Suspense>
```
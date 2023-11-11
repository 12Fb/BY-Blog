---
title: useContext
---
```useContext```可以让你读取和订阅组件中的 context。

- useContext允许你向组件树进行深度传值, 用```createContext```创建一个context, ``` const context = React.createContext(defaultValue);```,默认值处理一些兜底情况 
- 在顶层组件中Provide```<context.provider value={value} />  {children}  </context.provider>```
- 在接收的组件中使用useContext来进行接收 ```const myContext = useContext(context)```,接收的context**名字要对上**

 ### 潜在问题
  - createContext会在**不同的路由**中对一个相同的组件创建不同的context,虽然里面的数据是相同的,但是由于context是新的对象,会触发一次渲染,所有context的依赖项都会被重新渲染
  即使数据完全一样 **使用```useMemo```和```useCallback```来避免没有意义的重复渲染**

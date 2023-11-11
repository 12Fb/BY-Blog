---

---

```useCallback```是一个允许你在多次渲染中缓存的Hook,创建记忆化函数

## 参考
```useCallback(fn, dependencies)```
#### 参数 (官方原文)
- ```fn```: 想要缓存的函数。此函数可以接受任何参数并且返回任何值。React 将会在初次渲染而非调用时返回该函数。当进行下一次渲染时，如果 dependencies 相比于上一次渲染时没有改变，那么 React 将会返回相同的函数。否则，React 将返回在最新一次渲染中传入的函数，并且将其缓存以便之后使用。React 不会调用此函数，而是返回此函数。你可以自己决定何时调用以及是否调用。
- ```dependencies```: 有关是否更新 fn 的所有响应式值的一个列表。响应式值包括 props、state，和所有在你组件内部直接声明的变量和函数。如果你的代码检查工具 配置了 React，那么它将校验每一个正确指定为依赖的响应式值。依赖列表必须具有确切数量的项，并且必须像 [dep1, dep2, dep3] 这样编写。React 使用 Object.is 比较每一个依赖和它的之前的值。


### 用法
这里得引用一下```memo```,被他包裹的组件也是根据依赖进行缓存
来看一段代码:
```jsx
//子组件,接收一个函数
const son1 = memo(function son1({onChange:Function})) 
//父组件,接收更上层的一个theme参数
function changeTheme({theme}){
    const changeTheme = ()=>{
        ...
    }
    
    return <son1> onChange= {changeTheme} </son1>
}
```
上面的子组件用了```memo```来缓存,但是因为每次重新渲染的时候都会**生成一个新的changeTheme函数**,因此子组件的依赖-- changeTheme一直在变化,也就导致memo根本没有起到作用,这时候就需要```useCallback```来缓存
****
重写一下这段代码:
```jsx
function changeTheme({theme}){
    const changeTheme = useCallback((...args)=>{
       ...
    },args) //只要args没有变
    //不会重新创建changeTheme
    return <son1> onChange= {changeTheme} </son1>
}
```

#### 防止频繁触发useEffect
这是问题 -> useEffect中调用temp函数-> temp函数是useEffect依赖并且没有依赖项,创建新的temp ->触发useEffect依赖更新 -> 调用temp函数-> ... -> 死循环
  来看一下代码
  ```jsx
    function temp(){
        ...
    }
    useEffect(()=>{
        //被调用,同时是依赖,重新渲染会导致
        temp();
    },[temp])
  ```
  这个时候就有必要用```useCallback```来创建一个记忆化的temp,他传递了依赖<br/>
  **useEffect -> temp -> temp's dependencies 

#### 优化自定义Hook
如果你的Hook**返回了任何函数**,建议将他包裹到useCallback中,目的也是为了传递依赖


### 注意
- 如果```const myFunc=  useCallback(fn,[])```则不会将函数记忆化

  

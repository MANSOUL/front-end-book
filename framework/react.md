## React 

### 生命周期

自React 16.3开始，组件生命周期经过了比较大的改动。

对未来即将移除的三个生命周期添加 `UNSAFE_` 前缀：

- UNSAFE_componentWillMount()
- UNSAFE_componentWilReceiveProps()
- UNSAFE_componentWillUpdate()

并添加上新的生命周期：

- getDerivedStateFromProps()
- getSnapshotBeforeUpdate()

![React 生命周期](../images/4.jpg)


### 为什么要引入新的生命周期

为 React 的新 `Fiber` 算法做铺垫，**Fiber算法为了更高效的提升应用性能引入了异步渲染模式，这意味着在render函数之前的生命周期函数都可能会执行多次**。原先总会有开发者在生命周期函数中做一些副作用动作，例如在 `componentWillMount` 中进行 `ajax` 请求。

所以为了限制开发者在旧生命周期中调用 `this.setState()` 做一些将产生副作用的操作，因此引入了两个静态生命周期函数来替代旧生命周期函数。


### memo, PureComponent

### Portals

### Suspence

### Ref

### Diff 

### Fiber



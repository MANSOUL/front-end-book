## React 

### JSX 是什么？

JSX是一种用于表达XML等数据结构的一种书写方式，XML结构较为扁平且直观，而JSON数据格式能直接运行于JavaScript应用但相对XML不够直观，所以结构两者，便有了JSX语法。

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

如果组件是纯组件（同样的属性值传入会渲染出同样的结果），那么你对这个组件的期望当然是当下一次接受了相同的属性值时会去重用上一次的渲染结果，而不是再一次渲染。

但 React 默认不会执行这一操作,就像下述例子一样：

```jsx
import React from "react";
import "./styles.css";

function Title({ title }) {
  return <h1>{title}</h1>
}

export default function App() {
  const [count, setCount] = React.useState(0)
  return (
    <div className="App">
      <Title title="Hello Counter!"/>
      <p>{ count }</p>
      <button onClick={() => setCount(count+1)}>+1</button>
    </div>
  );
}
```

么次点击 `+1 button` 都会重新去渲染 `Title` 组件，这显然不是我们想要的结果。若要达到我们的目的，可以使用 `React.memo`。

```jsx
import React from "react";

function Title({ title }) {
  return <h1>{title}</h1>
}

const MemoTitle = React.memo(Title)

export default function App() {
  const [count, setCount] = React.useState(0)
  return (
    <div className="App">
      <MemoTitle title="Hello Counter!"/>
      <p>{ count }</p>
      <button onClick={() => setCount(count+1)}>+1</button>
    </div>
  );
}
```

现在，无论点击多少次按钮，都只会沿用上一次的渲染结果。

`PureComponent` 与 `React.memo` 都可以达到记忆化效果，区别在于 `PureComponent` 适用于类组件，而 `React.memo` 适用于函数组件。

### Portals

`Portrals` API通常用于将组件挂载到独立的DOM元素上，一个比较常见的例子就是模态框。

```jsx
import React from "react";

class Modal extends React.Component {
    render() {
        return React.createPortal(
            <div className="modal"></div>,
            document.body
        )
    }
}
```

### Ref

`ref` 可以创建类组件实例或DOM元素的引用，之后对他们进行访问。

创建引用的方式: 

- React.createRef
- React.useRef
- 通过向 `ref` 属性传递函数

```jsx
import React from 'react';

class Demo extends React.Component {
    constructor(props) {
        super(props);
        this.refTitle = React.createRef();
    }

    componentDidMount() {
        // ref.current => p
        console.log(this.refTitle.current);
    }

    render() {
        return (
            <p class="title" ref={this.refTitle}>Ref Demo</p>
        )
    }
}


function App () {
    const [refDemo, setRefDemo] = React.useRef(null)

    React.useEffect(() => {
        console.log(refDemo.current)
    })

    return (
        <div className="app">
            <Demo ref={refDemo}/>
        </div>
    )
}
```

### Suspence

`Suspence` 可以让组件在渲染之前“等待”某些东西。如今，Suspense仅支持一种用例：使用 `React.lazy` 动态加载组件。将来，它将支持其他用例，例如数据获取。

```jsx
const SomeComponent = React.lazy(() => import('./SomeComponent'));

function MyComponent() {
  return (
    // Displays <Spinner> until OtherComponent loads
    <React.Suspense fallback={<Spinner />}>
      <div>
        <SomeComponent />
      </div>
    </React.Suspense>
  );
}
```


### SSR

最初使用React书写的应用相对于原始应用，在SEO及首屏展示上都没有优势，由于React应用需要在JS下载并运行后才能渲染出真实的HTML结构，而不利于爬虫爬取，进而无法SEO优化。并且将会导致长时间的白屏，致使用户体验性降低。

好在借助于Node.js和React服务端渲染，这两个问题已经可以忽略不计了，前提是应用上服务端渲染。

对于React SSR，主要是借助于如下几个API实现。

1、`ReactDOM.hydrate()`

> 作用于 `render()` 一样，但是对于已经使用 `ReactDOMServer` 渲染出的HTML内容，只会为其添加事件监听。

2、`ReactDOMServer`

> 运行于Node服务端，用于将组件渲染为静态标记。

有如下几个方法：

**renderToString()**

> 将React元素渲染为HTML字符串，可以用这个方法在服务端生成HTML用于首屏优化和SEO。

> 如果在节点上调用了 `React.hydrate()`，React将仅会在这些已渲染的节点上添加事件。

**renderToStaticMarkup()**

> 和 `renderToString` 类似，但是不会渲染包含React内部所使用的额外的属性。可用于在React作为静态页面生成器的场景，使用这个方法可以省去一部分字节。

> 注意：使用这个方法渲染出的标记不具备交互性。

**renderToNodeStream**

> 作用同 `renderToString` 一样，不过不会将React元素渲染为字符串，而是一个可读流。

**renderToStaticNodeStream**

> 作用同 `renderToStaticMarkup` 一样，不过不会将React元素渲染为字符串，而是一个可读流。

### 组件通信

- 父子组件之间通过 `props`, `event prop` 通信
- 兄弟组件之间通过父组件作为桥梁通信
- 更深层的组件通过如果通过 `props` 通信，会导致许多组件需接收并处理很多无用的 `props`。因此可以通过 `context` 或第三方状态库通信。

### react router 原理

- `hash` 路由：通过改变路由上的hash值，并通过window监听 `hashchange` 变化实现
- `history` 模式：通过 `pushState`，`replaceState`及window监听 `popstate` 实现
- 虚拟路由：在非浏览器环境中通过内存模拟路由



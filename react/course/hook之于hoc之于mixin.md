---
theme: cyanosis
---
[外链文章](https://juejin.cn/post/6998395693861699598)
# 组件组合复用的方式
## Mixin设计模式
`Mixin`（混入）是一种通过扩展收集功能的方式，它本质上是将一个对象的属性拷贝到另一个对象上面去，不过你可以拷贝`任意多`个对象的`任意个`方法到一个新对象上去，这是`继承`所不能实现的。它的出现主要就是为了解决代码复用问题。 \
原理：

```js
function setMixin(target, mixin) {
  if (arguments[2]) {
    for (var i = 2, len = arguments.length; i < len; i++) {
      target.prototype[arguments[i]] = mixin.prototype[arguments[i]];
    }
  }
  else {
    for (var methodName in mixin.prototype) {
      if (!Object.hasOwnProperty(target.prototype, methodName)) {
        target.prototype[methodName] = mixin.prototype[methodName];
      }
    }
  }
}
setMixin(User,LogMixin,'actionLog');
setMixin(Goods,LogMixin,'requestLog');
```
这样以来，就可以使用`setMixin`方法将任意对象的任意方法扩展到目标对象上，实现代码复用功能
，但却带了了一些危害，如下：
-   `Mixin` 可能会相互依赖，相互耦合，不利于代码维护
-   不同的`Mixin`中的方法可能会相互冲突
-   `Mixin`非常多时，组件是可以感知到的，甚至还要为其做相关处理，这样会给代码造成滚雪球式的复杂性
[具体危害详情](https://react.docschina.org/blog/2016/07/13/mixins-considered-harmful.html)

## HOC模式
### 实现模式
 属性代理、 反向继承
### 属性代理和反向继承的对比
-   属性代理是从“组合”的角度出发，这样有利于从外部去操作 `WrappedComponent`，可以操作的对象是 `props`，或者在 `WrappedComponent` 外面加一些拦截器，控制器等。

-   反向继承则是从“继承”的角度出发，是从内部去操作 `WrappedComponent`，也就是可以操作组件内部的 `state` ，生命周期，`render`函数等等。

![WechatIMG124.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4c09bbc26dcb4b5ea231415511b464bc~tplv-k3u1fbpfcp-watermark.image)

-   可以看到，通过反向继承方法实现的高阶组件相较于属性代理实现的高阶组件，功能更强大，个性化程度更高，因此能适应更多的场景。

### HOC的缺陷

-   `HOC`需要在原组件上进行包裹或者嵌套，如果大量使用`HOC`，将会产生非常多的嵌套，这让调试变得非常困难。
-   `HOC`可以劫持`props`，在不遵守约定的情况下也可能造成冲突。

## 自定义hooks模式
### 使用Hook的动机
**减少状态逻辑复用的风险** \
`Hook`和`Mixin`在用法上有一定的相似之处，但是`Mixin`引入的逻辑和状态是可以相互覆盖的，而多个`Hook`之间互不影响，这让我们不需要在把一部分精力放在防止避免逻辑复用的冲突上。 \
在不遵守约定的情况下使用`HOC`也有可能带来一定冲突，比如`props`覆盖等等，使用`Hook`则可以避免这些问题。 \
**避免地狱式嵌套** \
大量使用`HOC`的情况下让我们的代码变得嵌套层级非常深，使用`Hook`，我们可以实现扁平式的状态逻辑复用，而避免了大量的组件嵌套。 \
**让组件更容易理解**  \
在使用`class`组件构建我们的程序时，他们各自拥有自己的状态，业务逻辑的复杂使这些组件变得越来越庞大，各个生命周期中会调用越来越多的逻辑，越来越难以维护。使用`Hook`，可以让你更大限度的将公用逻辑抽离，将一个组件分割成更小的函数，而不是强制基于生命周期方法进行分割。  \
**使用函数代替class**  \
相比函数，编写一个`class`可能需要掌握更多的知识，需要注意的点也越多，比如`this`指向、绑定事件等等。另外，计算机理解一个函数比理解一个`class`更快。`Hooks`让你可以在`classes`之外使用更多`React`的新特性。

### 传送门

[玩转react-hooks,自定义hooks设计模式及其实战](https://juejin.cn/post/6890738145671938062 "https://juejin.cn/post/6890738145671938062")

[react-hooks如何使用？](https://juejin.cn/post/6864438643727433741 "https://juejin.cn/post/6864438643727433741")

# 如何编写高阶组件
## 强化props
**有状态组件(属性代理)**

```js
function classHOC(WrapComponent){
    return class  Idex extends React.Component{
        state={
            name:'alien'
        }
        componentDidMount(){
           console.log('HOC')
        }
        render(){
            return <WrapComponent { ...this.props }  { ...this.state }   />
        }
    }
}
function Index(props){
  const { name } = props
  useEffect(()=>{
     console.log( 'index' )
  },[])
  return <div>
    hello,world , my name is { name }
  </div>
}

export default classHOC(Index)
```
##  控制渲染
###  条件渲染
对于属性代理的高阶组件，虽然不能在内部操控渲染状态，但是可以在外层控制当前组件是否渲染，这种情况应用于，**权限隔离**，**懒加载** ，**延时加载**等场景。

**实现一个动态挂载组件的HOC**

```js
function renderHOC(WrapComponent){
  return class Index  extends React.Component{
      constructor(props){
        super(props)
        this.state={ visible:true }  
      }
      setVisible(){
         this.setState({ visible:!this.state.visible })
      }
      render(){
         const {  visible } = this.state 
         return <div className="box"  >
           <button onClick={ this.setVisible.bind(this) } > 挂载组件 </button>
           { visible ? <WrapComponent { ...this.props } setVisible={ this.setVisible.bind(this) }   />  : <div className="icon" ><SyncOutlined spin  className="theicon"  /></div> }
         </div>
      }
  }
}

class Index extends React.Component{
  render(){
    const { setVisible } = this.props
    return <div className="box" >
        <p>hello,my name is alien</p>
        <img  src='https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=294206908,2427609994&fm=26&gp=0.jpg'   /> 
        <button onClick={() => setVisible()}  > 卸载当前组件 </button>
    </div>
  }
}
export default renderHOC(Index)
```
### 节流渲染
`hoc`可以配合`hooks`的`useMemo`等`API`配合使用，可以实现对业务组件的渲染控制，减少渲染次数，从而达到优化性能的效果。如下案例，我们期望当且仅当`num`改变的时候，渲染组件，但是不影响接收的`props`。我们应该这样写我们的`HOC`。

```js
function HOC (Component){
     return function renderWrapComponent(props){
       const { num } = props
       const RenderElement = useMemo(() =>  <Component {...props}  /> ,[ num ])
       return RenderElement
     }
}
class Index extends React.Component{
  render(){
     console.log(`当前组件是否渲染`,this.props)
     return <div>hello,world, my name is alien </div>
  }
}
const IndexHoc = HOC(Index)

export default ()=> {
    const [ num ,setNumber ] = useState(0)
    const [ num1 ,setNumber1 ] = useState(0)
    const [ num2 ,setNumber2 ] = useState(0)
    return <div>
        <IndexHoc  num={ num } num1={num1} num2={ num2 }  />
        <button onClick={() => setNumber(num + 1) } >num++</button>
        <button onClick={() => setNumber1(num1 + 1) } >num1++</button>
        <button onClick={() => setNumber2(num2 + 1) } >num2++</button>
    </div>
}
```
## 赋能组件
高阶组件除了上述两种功能之外，还可以赋能组件，比如加一些**额外`生命周期`**，**劫持事件**，**监控日志**等等。
### 劫持原型链-劫持生命周期，事件函数


**① 属性代理实现**

```js
function HOC (Component){
  const proDidMount = Component.prototype.componentDidMount 
  Component.prototype.componentDidMount = function(){
     console.log('劫持生命周期：componentDidMount')
     proDidMount.call(this)
  }
  return class wrapComponent extends React.Component{
      render(){
        return <Component {...this.props}  />
      }
  }
}
@HOC
class Index extends React.Component{
   componentDidMount(){
     console.log('———didMounted———')
   }
   render(){
     return <div>hello,world</div>
   }
}
```

**② 反向继承实现**

反向继承，因为在继承原有组件的基础上，可以对原有组件的**生命周期**或**事件**进行劫持，甚至是替换。
换。

```js
function HOC (Component){
  const didMount = Component.prototype.componentDidMount
  return class wrapComponent extends Component{
      componentDidMount(){
        console.log('------劫持生命周期------')
        if (didMount) {
           didMount.apply(this) /* 注意 `this` 指向问题。 */
        }
      }
      render(){
        return super.render()
      }
  }
}

@HOC
class Index extends React.Component{
   componentDidMount(){
     console.log('———didMounted———')
   }
   render(){
     return <div>hello,world</div>
   }
}
```
### 事件监控
`HOC`还可以对原有组件进行监控。比如对一些`事件监控`，`错误监控`，`事件监听`等一系列操作。 \
如组件内的事件监听

```js
function ClickHoc (Component){
  return  function Wrap(props){
    const dom = useRef(null)
    useEffect(()=>{
     const handerClick = () => console.log('发生点击事件') 
     dom.current.addEventListener('click',handerClick)
     return () => dom.current.removeEventListener('click',handerClick)
    },[])
    return  <div ref={dom}  ><Component  {...props} /></div>
  }
}

@ClickHoc
class Index extends React.Component{
   render(){
     return <div  className='index'  >
       <p>hello，world</p>
       <button>组件内部点击</button>
    </div>
   }
}
export default ()=>{
  return <div className='box'  >
     <Index />
     <button>组件外部点击</button>
  </div>
}
```

# Hooks 会取代渲染道具和高阶组件吗？
官方的回答是，在 HoC 或者 Render Props 只有一个 child 的情况下通常 hooks 是一个更简单的提供的方式。但我想，我们可以再深挖一下。
[原文](https://reactjs.org/docs/hooks-faq.html#do-hooks-replace-render-props-and-higher-order-components)

在 Hooks 之前，如果要做不同注入数据/状态之间的计算是挺操蛋的。

HoC 方案的典型思路是，在对原视图进行多次的 decorate 之后，再进行 mapProps 处理，将前面的多个数据进行一次中心化的计算。这是个特别函数式的操作，可能有一定的门槛；而且，这种方式的确与 render 距离太远了，以至于在 class 内部可能都得纠结一下各个 props 究竟是来自于哪里，如果有问题、问题又出自于哪里。

而另一种思路则是 Render Props, 就是把 HoC 包装为一个 JSX 组件，通过 function children 或者 render props 将状态递交给下一层，典型就像 motion 库中做的那样；问题就在于如果有很多的组件依赖，就会一层套一层地形成 JSX 中的嵌套地狱。而且，这种方式显然太深入到 render 过程中了，一些简单的操作却需要嵌套一层 JSX 也是挺痛苦的。

而上面不管是哪种方式，都还是将各个数据 / 状态之间的关系给拉远了。

Hooks 就解决了这个问题，就像 async / await 之于 Promise 的关系那样。


但这显然并不意味着 Hooks 就能取代了 HoC 以及 Render Props，因为他们其实还是有着各自的优势所在。

-   HoC 可以做到很轻松地**外部协议化注入**功能到一个基础 Component 中，所以可以用来做插件。就像 react-swipeable-views 中的 autoPlay HoC 那样，这个功能特性直接放到主库里面感觉不太合适，但可以通过注入状态化的 props 的方式进行扩展。这种特性使得 HoC 特别适合用来做基础组件的**插件**，也适合用于做 Adapter 层。而反观 Hooks 中间的处理过程则一定会与目标组件强依赖。这不是 Hooks 的缺陷，但 Hooks 显然并不是设计来进行插件注入的。
-   HoC 的方式可以天然地进行组件的**分层以及组合**，并且这种分层基本都可以描述为状态注入以及 props mapping 的过程。并且，这个分层过程可以描述为函数式的 compose，这对于其中的配置及顺序管理非常有利。
-   而看看 Render Props，其使用时天然的就是 JSX 的一部分，那就意味着他JSX 中的逻辑计算来进行其中参数的管控，也可以通过 VDOM 中的 diff 特性提供性能上的优化。虽然嵌套地狱似乎不是什么好事，但**树状结构**本身就是嵌套的，XML Like 的语法却依然更合适用于描述树状结构以及其中的决策过程。
-   Render Props 还是非常天然而简单的逻辑，在其中可以放心大胆地进行各种 map, if-else, && 等各类操作。而 Hooks 却不是，他的设计机制使得使用起来与调用顺序强相关，那限制就多。

所以，Hooks 个人看来更多是对原有的 HoC 以及 Render Props 方案的**补充**，填补了原本两边都不擅长的部分。他的写法天然地可以让代码更加紧凑，更适合做 **Controller** 或者需要**内聚**的相关逻




















先学习react
在学习ts
在学习vue3.0

# 什么是函数式编程
- 不可变性(Immutability)
- 纯函数(Pure Functions)
     > 纯函数是始终接受一个或多个参数并计算参数并返回数据或函数的函数。 它没有副作用，例如设置全局状态，更改应用程序状态，它总是将参数视为不可变数据。
- 数据转换(Data Transformations)
- 高阶函数 (Higher-Order Functions)
  >高阶函数是将函数作为参数或返回函数的函数，或者有时它们都有。 这些高阶函数可以操纵其他函数。
- 递归
- 组合

# 组件和不同类型
- 函数/无状态/展示组件
  >函数或无状态组件是一个纯函数，它可接受接受参数，并返回react元素。这些都是没有任何副作用的纯函数。

- 类/有状态组件
  >类或有状态组件具有状态和生命周期方可能通过setState()方法更改组件的状态。类组件是通过扩展React创建的。
- 受控组件
  >受控组件是在 React 中处理输入表单的一种技术。表单元素通常维护它们自己的状态，而react则在组件的状态属性中维护状态。我们可以将两者结合起来控制输入表单。这称为受控组件。因此，在受控组件表单中，数据由React组件处理。
- 非受控组件
  >大多数情况下，建议使用受控组件。有一种称为非受控组件的方法可以通过使用Ref来处理表单数据。在非受控组件中，Ref用于直接从DOM访问表单值，而不是事件处理程序。
- 容器组件
  >容器组件是处理获取数据、订阅 redux 存储等的组件。它们包含展示组件和其他容器组件，但是里面从来没有html。
- 高阶组件
  > 高阶组件是将组件作为参数并生成另一个组件的组件。 Redux connect是高阶组件的示例。 这是一种用于生成可重用组件的强大技术。
  
# 什么是 PropTypes
PropTypes为组件提供类型检查，并为其他开发人员提供很好的文档。如果react项目不使用 Typescript，建议为组件添加 PropTypes。
# 如何更新状态以及如何不更新
js
// 正确方式
this.setState((state, props) => {
    timesVisited: state.timesVisited + props.count
});
# 组件生命周期方法
componentWillMount()
componentDidMount()
componentWillReceiveProps()
shouldComponentUpdate()
# 什么是Redux及其工作原理

# 超越继承的组合
# 什么是 Hooks
# 如何在重新加载页面时保留数据

# 如何在React进行API调用


什么是组件设计模式

什么是虚拟DOM及其工作原理





超越继承的组合
如何在React中应用样式
什么是Redux及其工作原理
什么是React路由器及其工作原理
什么是错误边界
什么是 Fragments
什么是传送门(Portals)
什么是 Context

如何提高性能
如何在重新加载页面时保留数据
如何从React中调用API


1、Vue 官方对比
Vue 官方对比 React[https://cn.vuejs.org/v2/guide/comparison.html]

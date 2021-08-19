# 代码分割
在你的应用中引入代码分割的最佳方式是通过动态 `import()` 语法。

当 Webpack 解析到该语法时，会自动进行代码分割。如果你使用 Create React App，该功能已开箱即用，你可以[立刻使用](https://facebook.github.io/create-react-app/docs/code-splitting)该特性。[Next.js](https://nextjs.org/docs/advanced-features/dynamic-import) 也已支持该特性而无需进行配置。

如果你自己配置 Webpack，你可能要阅读下 Webpack 关于[代码分割](https://webpack.docschina.org/guides/code-splitting/)的指南。你的 Webpack 配置应该[类似于此](https://gist.github.com/gaearon/ca6e803f5c604d37468b0091d9959269)。

当使用 [Babel](https://babel.docschina.org/) 时，你要确保 Babel 能够解析动态 import 语法而不是将其进行转换。对于这一要求你需要 [babel-plugin-syntax-dynamic-import](https://yarnpkg.com/en/package/babel-plugin-syntax-dynamic-import) 插件。
## `React.lazy`
```js
const OtherComponent = React.lazy(() => import('./OtherComponent'));
```
此代码将会在组件首次渲染时，自动导入包含 `OtherComponent` 组件的包。

```js
import React, { Suspense } from 'react';

const OtherComponent = React.lazy(() => import('./OtherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <OtherComponent />
      </Suspense>
    </div>
  );
}
```
## 异常捕获边界（Error boundaries）
```js
import React, { Suspense } from 'react';
import MyErrorBoundary from './MyErrorBoundary';

const OtherComponent = React.lazy(() => import('./OtherComponent'));
const AnotherComponent = React.lazy(() => import('./AnotherComponent'));

const MyComponent = () => (
  <div>
    <MyErrorBoundary>
      <Suspense fallback={<div>Loading...</div>}>
        <section>
          <OtherComponent />
          <AnotherComponent />
        </section>
      </Suspense>
    </MyErrorBoundary>
  </div>
);
```
## 基于路由的代码分割

```js
import React, { Suspense, lazy } from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';

const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={Home}/>
        <Route path="/about" component={About}/>
      </Switch>
    </Suspense>
  </Router>
);
```
# Context
Context 提供了一个无需为每层组件手动添加 props，就能在组件树间进行数据传递的方法。
## 何时使用 Context
Context 设计目的是为了共享那些对于一个组件树而言是“全局”的数据，例如当前认证的用户、主题或首选语言。举个例子，在下面的代码中，我们通过一个 “theme” 属性手动调整一个按钮组件的样式：
```js
// Context 可以让我们无须明确地传遍每一个组件，就能将值深入传递进组件树。
// 为当前的 theme 创建一个 context（“light”为默认值）。
const ThemeContext = React.createContext('light');
class App extends React.Component {
  render() {
    // 使用一个 Provider 来将当前的 theme 传递给以下的组件树。    
    // 无论多深，任何组件都能读取这个值。   
    // 在这个例子中，我们将 “dark” 作为当前的值传递下去。   
    return (
      <ThemeContext.Provider value="dark">      
          <Toolbar />
      </ThemeContext.Provider>
    );
  }
}

// 中间的组件再也不必指明往下传递 theme 了。function Toolbar() {  return (
    <div>
      <ThemedButton />
    </div>
  );
}

class ThemedButton extends React.Component {
  // 指定 contextType 读取当前的 theme context。  
  // React 会往上找到最近的 theme Provider，然后使用它的值。 
  // 在这个例子中，当前的 theme 值为 “dark”。  
  static contextType = ThemeContext;
  render() {
    return <Button theme={this.context} />;  }
}
```
## 使用 Context 之前的考虑
Context 主要应用场景在于*很多*不同层级的组件需要访问同样一些的数据。请谨慎使用，因为这会使得组件的复用性变差。

**如果你只是想避免层层传递一些属性，[组件组合（component composition）](https://react.docschina.org/docs/composition-vs-inheritance.html)有时候是一个比 context 更好的解决方案。**
## API
### `React.createContext`
### `Context.Provider`
### `Class.contextType`
# 错误边界
错误边界是一种 React 组件，这种组件**可以捕获并打印发生在其子组件树任何位置的 JavaScript 错误，并且，它会渲染出备用 UI**，而不是渲染那些崩溃了的子组件树。错误边界在渲染期间、生命周期方法和整个组件树的构造函数中捕获错误。
注意

错误边界**无法**捕获以下场景中产生的错误：

-   事件处理（[了解更多](https://react.docschina.org/docs/error-boundaries.html#how-about-event-handlers)）
-   异步代码（例如 `setTimeout` 或 `requestAnimationFrame` 回调函数）
-   服务端渲染
-   它自身抛出来的错误（并非它的子组件）
# Refs 转发











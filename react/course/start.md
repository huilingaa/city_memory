# React 团队主要推荐这些解决方案：

-   如果你是在**学习 React** 或**创建一个新的[单页](https://react.docschina.org/docs/glossary.html#single-page-application)应用**，请使用 [Create React App](https://react.docschina.org/docs/create-a-new-react-app.html#create-react-app)。
-   如果你是在**用 Node.js 构建服务端渲染的网站**，试试 [Next.js](https://react.docschina.org/docs/create-a-new-react-app.html#nextjs)。
-   如果你是在构建**面向内容的静态网站**，试试 [Gatsby](https://react.docschina.org/docs/create-a-new-react-app.html#gatsby)。
-   如果你是在打造**组件库**或**将 React 集成到现有代码仓库**，尝试[更灵活的工具链](https://react.docschina.org/docs/create-a-new-react-app.html#more-flexible-toolchains)。

以下工具链为 React 提供更多更具灵活性的方案。推荐给更有经验的使用者：

-   **[Neutrino](https://neutrinojs.org/)** 把 [webpack](https://webpack.js.org/) 的强大功能和简单预设结合在一起。并且包括了 [React 应用](https://neutrinojs.org/packages/react/)和 [React 组件](https://neutrinojs.org/packages/react-components/)的预设。
-   **[Parcel](https://parceljs.org/)** 是一个快速的、零配置的网页应用打包器，并且可以[搭配 React 一起工作](https://parceljs.org/recipes.html#react)。
-   **[Razzle](https://github.com/jaredpalmer/razzle)** 是一个无需配置的服务端渲染框架，但它提供了比 Next.js 更多的灵活性。
## 从头开始打造工具链

一组 JavaScript 构建工具链通常由这些组成：

-   一个 **package 管理器**，比如 [Yarn](https://yarnpkg.com/) 或 [npm](https://www.npmjs.com/)。它能让你充分利用庞大的第三方 package 的生态系统，并且轻松地安装或更新它们。
-   一个**打包器**，比如 [webpack](https://webpack.js.org/) 或 [Parcel](https://parceljs.org/)。它能让你编写模块化代码，并将它们组合在一起成为小的 package，以优化加载时间。
-   一个**编译器**，例如 [Babel](https://babeljs.io/)。它能让你编写的新版本 JavaScript 代码，在旧版浏览器中依然能够工作。

如果你倾向于从头开始打造你自己的 JavaScript 工具链，可以[查看这个指南](https://blog.usejournal.com/creating-a-react-app-from-scratch-f3c693b84658)，它重新创建了一些 Create React App 的功能。

别忘了确保你自定义的工具链[针对生产环境进行了正确配置](https://react.docschina.org/docs/optimizing-performance.html#use-the-production-build)。


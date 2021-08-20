
**Github 仓库** : [vue-cli](https://github.com/vuejs/vue-cli)

![WechatIMG429.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2f215afbb38d4c7e92bf668adcef6e15~tplv-k3u1fbpfcp-watermark.image)
# 常见 npm 包
-   [commander](https://github.com/tj/commander.js)：提供了用户命令行输入和参数解析强大功能。
-   [Inquirer](https://github.com/SBoudrias/Inquirer.js)：交互式命令行工具
-   [execa](https://github.com/sindresorhus/execa)：A better [child_process](https://nodejs.org/api/child_process.html)。
-   [handlebars](https://github.com/wycats/handlebars.js)：一个 javascript 语以模版库。
-   [metalsmith](https://github.com/segmentio/metalsmith)；An extremely simple, pluggable static site generator。
-   [chalk](https://github.com/chalk/chalk)：Terminal string styling done right。
-   [download-git-repo](https://github.com/flipxfx/download-git-repo)：Download and extract a git repository (GitHub, GitLab, Bitbucket) from node。
-   [consolidate](https://github.com/tj/consolidate.js)：Template engine consolidation library for node.js 。
从[官方文档](https://link.segmentfault.com/?url=https%3A%2F%2Fgithub.com%2Fvuejs%2Fvue-cli%2Fblob%2Fmaster%2FREADME.md)可以知道，vue-cli使用了[download-git-repo](https://link.segmentfault.com/?url=https%3A%2F%2Fgithub.com%2Fflipxfx%2Fdownload-git-repo)这个工具去下载远端git仓库里面的工程，如果加上了`--clone`参数，则会在内部运行`git clone`，通过克隆的方式把远端git仓库拉取到本地。
# 传送门：  
[vue-cli源码分析](https://kuangpf.com/vue-cli-analysis/info/)  \
[教你从零开始搭建一款前端脚手架工具](https://segmentfault.com/a/1190000006190814)  \
[插件开发指南](https://cli.vuejs.org/zh/dev-guide/plugin-dev.html)










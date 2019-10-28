# Electron 入门

## 初始化项目阶段

- 项目的依赖 `npm install` 安装成功率比较低，网络的问题
- 可以把下载的依赖 `node_modules` 全部删除再下载试试，真不行看着报错提示安装对应的依赖

## 知识来源

> 笔记信息源 [Electron-vue 开发实战](https://molunerfinn.com/electron-vue-1/)

## 什么是 Electron

![img](http://pzxqru49l.bkt.clouddn.com/images/3zd.png)

### 图解

electron 由 Node.js+Chromium+Native APIs 构成。你可以理解成，它是一个得到了 Node.js 和基于不同平台的 Native APIs 加强的 Chromium 浏览器，可以用来开发跨平台的桌面级应用。

它的开发主要涉及到两个进程的协作——Main（主）进程和 Renderer（渲染）进程。简单的理解两个进程的作用：

1. Main 进程主要通过 Node.js、Chromium 和 Native APIs 来实现一些系统以及底层的操作，比如创建系统级别的菜单，操作剪贴板，创建 APP 的窗口等。
2. Renderer 进程主要通过 Chromium 来实现 APP 的图形界面——就是平时我们熟悉的前端开发的部分，不过得到了 electron 给予的加强，一些 Node 的模块（比如 fs）和一些在 Main 进程里能用的东西（比如 Clipboard）也能在 Render 进程里使用。
3. Main 进程和 Renderer 进程通过`ipcMain`和`ipcRenderer`来进行通信。通过事件监听和事件派发来实现两个进程通信，从而实现 Main 或者 Renderer 进程里不能实现的某些功能。

### 进一步介绍

说完了 electron 的组成和需要我们开发的部分，来说说它的优缺点。

优点：

1. 从上述介绍可以发现，除了不同平台 Native APIs 不同以外，Node.js 和 Chromium 都是跨平台的工具，这也为 electron 生来就能做跨平台的应用开发打下基础。
2. 开发图形界面前所未有的容易——比起 C#\QT\MFC 等传统图形界面开发技术，通过前端的图形化界面开发明显更加容易和方便。得益于 Chromium，这种开发模式得以实现。
3. 成熟的社区、活跃的核心团队，大部分 electron 相关的问题你可以在社区、github issues、Stack Overflow 里得到答案。开发的障碍进一步降低。

缺点：

1. 应用体积过大。由于内部包装了 Chromium 和 Node.js，使得打包体积（使用`electron-builder`）在 mac 上至少是 45M+起步，在 windows 上稍微好一点，不过也要 35M+起步。不过相比早期打包体积 100M+起步来说，已经好了不少。不过解压后安装依然是 100M+起步。
2. 受限于 Node.js 和 Native APIs 的一些支持度的问题，它依然有所局限。一些功能依然无法实现。比如无法获取在系统文件夹里选中的文件，而必须调用 web 的 File 或者 node 的 fs 接口才可以访问系统文件。
3. 应用性能依旧是个问题。所以做轻量级应用没问题，重量级应用尤其是 CPU 密集型应用的话很是问题。

## 何为 electron-vue

[electron-vue](https://github.com/SimulatedGREG/electron-vue)，这个是开发者[SimulatedGREG](https://github.com/SimulatedGREG)参考 vue-cli 的 webpack 模板骨架搭建的 electron 和 vue 结合的开发脚手架。由于我对于`vue-cli`非常熟悉，所以上手`electron-vue`非常容易。相比很多其他的教程或者其他 electron+前端开发框架的组装方案，`electron-vue`给我感觉最好的是如下：

1. 只有一个`package.json`。而大部分其他的项目结构依然在使用两个`package.json`来应对 main 进程和 renderer 进程的依赖库。
2. 内建完整的 vue 全家桶，省去再次配置 `vue-router` 和 `vuex` 的一些初期操作。
3. 内建完整的 webpack 开发、生产等配置，开发环境舒适。
4. 内建完整的开发、构建等`npm scripts`，使用非常方便。
5. 内建完整的 Travis-ci、Appveyor 配置脚本，只需少数修改就能做到利用 CI 自动构建的应用发布。
6. 完善的文档，清晰的项目结构。

大体的项目结构如下，根据选择的不同设置结构会有所不同：

```bash
my-project
├─ .electron-vue
│  └─ <build/development>.js files
├─ build
│  └─ icons/
├─ dist
│  ├─ electron/
│  └─ web/
├─ node_modules/
├─ src
│  ├─ main # 主进程
│  │  ├─ index.dev.js
│  │  └─ index.js
│  ├─ renderer # 渲染进程
│  │  ├─ components/
│  │  ├─ router/
│  │  ├─ store/
│  │  ├─ App.vue
│  │  └─ main.js
│  └─ index.ejs
├─ static/
├─ test
│  ├─ e2e
│  │  ├─ specs/
│  │  ├─ index.js
│  │  └─ utils.js
│  ├─ unit
│  │  ├─ specs/
│  │  ├─ index.js
│  │  └─ karma.config.js
│  └─ .eslintrc
├─ .babelrc
├─ .eslintignore
├─ .eslintrc.js
├─ .gitignore
├─ package.json
└─ README.md
```

我们主要关注的两个文件夹：`src/main` 和 `src/renderer` 分别对应的是 main 进程和 renderer 进程。开发大体上也是围绕这两个文件夹展开。

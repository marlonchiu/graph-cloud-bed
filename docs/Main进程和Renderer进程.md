# Main 进程和 Renderer 进程

## 知识来源

> 笔记信息源 [Main 进程和 Renderer 进程的基本认识](https://molunerfinn.com/electron-vue-2/)

## Main 进程和 Renderer 进程的基本认识

Main 进程管理的是这个 app 窗口（[BrowserWindow](https://electronjs.org/docs/api/browser-window)），而 Renderer 进程负责的就是我们熟悉的页面 UI 渲染。
![main & renderer process tree](http://pzxqru49l.bkt.clouddn.com/images/8700af19ly1fnhc.png)

它们有各自的模块，也有共有的模块比如 clipboard 等。还有一部分是 Main 进程里的模块，不过可以通过 remote 模块，让 renderer 进程也能使用。比如 Menu 比如 shell 等。

了解一下哪些模块在哪些进程里，哪些模块可以通过 remote 模块让 renderer 进程也能使用是有必要的，这样我们后续开发的时候才能正确的使用。

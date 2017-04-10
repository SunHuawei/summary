[JavaScript模块演化简史](https://zhuanlan.zhihu.com/p/26231889?group_id=833718460298190848)的**摘要**


# JavaScript 模块化

早期的JavaScript并没有模块化解决方案。
随着单页应用与富客户端的流行，出现了模块化（Modularity）。

模块化主要是解决

1. 代码分割
2. 作用域隔离
3. 模块之间的依赖管理
4. 发布到生产环境时的自动化打包与处理

流行过的 JavaScript 模块化解决方案包括但不限于

1. 直接声明依赖（Directly Defined Dependences）
2. 命名空间（Namespace Pattern）
3. 模块模式（Module Pattern）
4. 依赖分离定义（Detached Dependency Definitions）
5. 沙盒（Sandbox）
6. 依赖注入（Dependency Injection）
7. CommonJS、AMD、UMD
8. 标签化模块（Labeled Modules）
9. YModules
10. ES 2015 Modules

早期web开发中，很容易遇到的问题是**命名冲突（Name Collisions）**。

此外应用变大后

1. 无法将所有代码写到单个JavaScript文件中
2. 手动引入所有脚本文件也是个问题
3. 特别是存在着模块间依赖的问题

物极必反，过度碎片化的模块同样会带来性能的损耗与包体尺寸的增大，这包括了

1. 模块加载
2. 模块解析
3. JavaScript 引擎优化失败(因为 Webpack 等打包工具包裹模块时封装的过多 IIFE 函数导致的)


# 命名空间模式

1. 统一添加前缀，如myApp_address, myApp_validateUser()
2. 对象封装，将所有相关变量封装入全局对象中

对象封装的缺点是

1. 大型多人协同项目的可维护性还是较差
2. 没有解决模块间依赖管理的问题


# 依赖注入

依赖注入的核心思想在于某个模块**不需要手动地初始化**某个依赖对象，而**只需要声明**该依赖并由外部框架自动实例化该对象实现并且传递到模块内。
Angular／Angular2／Slot中依赖注入是核心机制之一。


# CommonJS

主要包含`require` 与 `module` 这两个关键字。

CommonJS 算是目前最流行的模块格式，可以应用在

1. Node.js
2. [Browserify**](http://link.zhihu.com/?target=http%3A//browserify.org/)（客户端）
3. [Webpack**](http://link.zhihu.com/?target=https%3A//webpack.js.org/)（客户端）

需要注意的是，**Node.js 中的模块在加载之后是以单例化运行，并且遵循值传递原则**


# AMD(Asynchronous Module Definition)

实现了**异步加载模块**。目的是**充分利用浏览器的并发加载能力**。
主要包含`require` 与 `define` 这两个关键字。

> 随着以 npm 为主导的依赖管理机制的统一，越来越多的开发者放弃了使用 AMD 模式。


# UMD(Universal Module Definition)

同时支持 CommonJS 与 AMD 规范的模块。


# ES2015 Modules

主要包含`import` 与 `export` 这两个关键字。

> 我们目前还不能直接保证在所有环境（特别是旧版本浏览器）中使用该规范，但是通过 Babel 等转化工具能帮我们自动处理向下兼容。

ES2015 Modules 还是有些许被诟病的地方

1. 导入语句只能作为模块顶层的语句出现，不能出现在 function 里面或是 if 里面
2. 并且 import 语句会被提升到文件顶部执行，也就是说在模块初始化的时候所有的 import 都必须已经导入完成
3. import 的模块名只能是字符串常量，导入的值也是不可变对象

> 这些设计虽然使得灵活性不如 CommonJS 的 require，但却保证了 ES6 Modules 的依赖关系是确定（Deterministic）的，和运行时的状态无关，从而也就保证了 ES6 Modules 是可以进行可靠的静态分析的。

浏览器中默认支持 ES2015 Modules的情况请查看原文。
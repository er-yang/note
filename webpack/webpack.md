

# webpack

**webpack** 是一个用于现代 JavaScript 应用程序的_静态模块打包工具_。当 webpack 处理应用程序时，它会在内部构建一个依赖图dependency graph，此依赖图对应映射到项目所需的每个模块，并生成一个或多个 *bundle*

其核心概念

- [入口(entry)](https://webpack.docschina.org/concepts/#entry)

- [输出(output)](https://webpack.docschina.org/concepts/#output)

- [loader](https://webpack.docschina.org/concepts/#loaders)用于对模块的源代码进行转换

- [插件(plugin)](https://webpack.docschina.org/concepts/#plugins)用于完成loader不能实现的其他事，在生成compiler对象时就会读取所有的plugin，实例化并挂载在webpack的事件机制中

- **compiler** 对象代表了完整的 webpack 环境配置。这个对象在启动 webpack 时被一次性建立，并配置好所有可操作的设置，包括 options，loader 和 plugin

- **competition**由compiler通过 [CLI](https://www.webpackjs.com/api/cli) 或 [Node API](https://www.webpackjs.com/api/node) 传递的所有选项创建出的一个对象实例，对象代表了一次资源版本构建

  

## loader

loader 是导出为一个函数的 node 模块，该函数在 loader 转换资源的时候调用。给定的函数将调用 ，并通过 `this` 上下文访问。

eg：

```js
export default function(source) {
  const options = getOptions(this);

  validateOptions(schema, options, 'Example Loader');

  // 对资源应用一些转换……

  return `export default ${ JSON.stringify(source) }`;
};
```

 loader 只能传入一个参数 - 这个参数是一个包含包含资源文件内容的字符串，可通过返回值return或者调用`this.callback(err, values...)` 函数，返回任意数量的值。

因此同步 loader 可以简单的返回一个代表模块转化后的值，异步loader可通过callback返回

## plugin

插件由以下组成：

- 一个 JavaScript 命名函数。

- 在插件函数的 prototype 上定义一个 `apply` 方法。

- 指定一个绑定到 webpack 自身的[事件钩子](https://www.webpackjs.com/api/compiler-hooks/)。

- 处理 webpack 内部实例的特定数据。

- 功能完成后调用 webpack 提供的回调

```javascript
  // 一个 JavaScript 命名函数。
  function MyExampleWebpackPlugin() {
  
  };
  
  // 在插件函数的 prototype 上定义一个 `apply` 方法。
  MyExampleWebpackPlugin.prototype.apply = function(compiler) {
    // 指定一个挂载到 webpack 自身的事件钩子。
    compiler.plugin('webpacksEventHook', function(compilation /* 处理 webpack 内部实例的特定数据。*/, callback) {
      console.log("This is an example plugin!!!");
  
      // 功能完成后调用 webpack 提供的回调。
      callback();
    });
    
  module.exports = MyExampleWebpackPlugin;
```


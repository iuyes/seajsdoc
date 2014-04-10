[Permalink](http://seajs.org/docs/ "Permalink to A Module Loader for the Web")

# A Module Loader for the Web

## Loading ...

## 提供简单、极致的模块化开发体验

A Module Loader for the Web

## 5 分钟上手 Sea.js

这是个小游戏，调皮的字母来自神秘的大海深处。当鼠标轻轻滑过时，字母会旋转到正确位置。
注意：该例子仅在高级浏览器下有效，推荐用 Chrome 浏览。
来试试吧，看能否让所有字母都听话……

下面花 5 分钟时间，来看看这个小项目如何实现。

### 目录结构

所有源码都存放在 GitHub 上：[seajs/examples][1]，目录结构为：


    examples/
      |-- sea-modules      存放 seajs、jquery 等文件，这也是模块的部署目录
      |-- static           存放各个项目的 js、css 文件
      |     |-- hello
      |     |-- lucky
      |     `-- todo
      `-- app              存放 html 等文件
            |-- hello.html
            |-- lucky.html
            `-- todo.html


我们从 `hello.html` 入手，来瞧瞧使用 Sea.js 如何组织代码。

### 在页面中加载模块

在 `hello.html` 页尾，通过 `script` 引入 `sea.js` 后，有一段配置代码：


    // seajs 的简单配置
    seajs.config({
      base: "../sea-modules/",
      alias: {
        "jquery": "jquery/jquery/1.10.1/jquery.js"
      }
    })

    // 加载入口模块
    seajs.use("../static/hello/src/main")


`sea.js` 在下载完成后，会自动加载入口模块。页面中的代码就这么简单。

### 模块代码

这个小游戏有两个模块 `spinning.js` 和 `main.js`，遵循统一的写法：


    // 所有模块都通过 define 来定义
    define(function(require, exports, module) {

      // 通过 require 引入依赖
      var $ = require('jquery');
      var Spinning = require('./spinning');

      // 通过 exports 对外提供接口
      exports.doSomething = ...

      // 或者通过 module.exports 提供整个接口
      module.exports = ...

    });


上面就是 Sea.js 推荐的 CMD 模块书写格式。如果你有使用过 Node.js，一切都很自然。

### 构建部署

对于正式项目，在发布上线前，还需要对源码进行压缩、合并等操作。
这可以通过 spm 或 Grunt 等工具来实现。简明教程请参考：[构建工具][2]

### 结束语

怎么样，Sea.js 入门真的只需 5 分钟吧：）

使用 Sea.js，可以规范模块的书写格式、能自动处理模块的依赖，还非常有助于代码组织、开发调试和性能优化。Sea.js 期待能给你提供简单、极致的模块化开发体验。我相信，你会爱上她的。

若喜欢，可查看更多例子：[seajs/examples][3]
若已爱上，请继续阅读：[使用文档][4]

有任何疑问，欢迎交流：[社区][5]

   [1]: https://github.com/seajs/examples
   [2]: https://github.com/seajs/seajs/issues/538
   [3]: http://seajs.github.io/examples
   [4]: http://seajs.org#docs
   [5]: https://github.com/seajs/seajs/issues/271

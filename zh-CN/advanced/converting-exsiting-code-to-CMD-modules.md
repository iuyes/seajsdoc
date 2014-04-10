[Permalink](https://github.com/seajs/seajs/issues/971 "Permalink to 如何改造现有文件为 CMD 模块 · Issue #971 · seajs/seajs · GitHub")

# 如何改造现有文件为 CMD 模块 · Issue #971 · seajs/seajs · GitHub

经过一段考察，我们终于要在项目中引入模块机制和 Sea.js 了，那么如何将现有的文件改造成 CMD 模块就成了重要的问题。下面针对一些典型场景来说明包装的方式。

首先还是请大家详细了解下 [CMD 模块定义规范][1]，只要洞悉事物的定义和本质，一切问题可迎刃而解。

## 改造主流模块

这里指的是 jQuery、Moment、Backbone、underscore 等业界主流模块，这些模块一般都有对 AMD 和 CommonJS 的支持代码，例如 jQuery 源文件的最后几行：


    if ( typeof module === "object" &amp;&amp; module &amp;&amp; typeof module.exports === "object" ) {
        module.exports = jQuery;
    } else {
        window.jQuery = window.$ = jQuery;

        if ( typeof define === "function" &amp;&amp; define.amd ) {
            define( "jquery", [], function () { return jQuery; } );
        }
    }


还有 Backbone 里：


    var Backbone;
    if (typeof exports !== 'undefined') {
      Backbone = exports;
    } else {
      Backbone = root.Backbone = {};
    }


对于有这些兼容代码的，只需要去掉 define.amd 的支持，或是直接包装上 `define` 就可以了。


    define(function(require, exports, module) {
      // code here ...
    });


如果没有模块化的兼容代码，有时候需要手动引入依赖，以及暴露对应的接口。


    define(function(require, exports, module) {
      var $ = require('$');
      // code here ...
      module.exports = Placeholders;
    });


可以参考 [cmdjs/gallery][2] 里的 [Gruntfile][3] 对这些主流模块的代码替换方式。

## 现有的 JS 文件

对于一些现有的普通 JS 文件，相对简单的多，参考 CMD 的[书写规范][1]，把那些暴露到全局命名空间的接口用 CMD 的方式进行改造就可以了。

比如现有文件 `util.js` 。


    window.util = window.util || {};
    util.addClass = function() {
      window.css();
    };
    util.queryString = function() {};


改为：


    define(function(require, exports, module) {
      // 引入依赖
      var css = require('css');

      util.addClass = function() {
        css();
      };
      util.queryString = function() {};

      // 暴露对应接口
      module.exports = util;
    });


这里不啰嗦，就是基本的 CMD 书写规范。实际的改造过程中，情况可以比上面的例子要复杂一些，具体情况具体解决就行。

## 改造 jQuery 插件

这也是一个经常遇到的问题，我们推荐以下的包装方式：


    // jquery-plugin-abc
    define(function(require, exports, module) {
      var $ = require('$');
      // 插件的代码
      $.fn.abc = function() {};
    });


这样的改造方式实际上是强化了原有的 $ 对象（而不是重新包装出一个新的 $），在实际调用时，可以用下面的方式：


    seajs.use(['$', 'jquery-plugin-abc'], function($) {
      // $ 有了 jquery-plugin-abc 所提供的插件功能
      $.abc();
    });


更多 jQuery 插件的包装，可以参考 [cmdjs/jquery][4] 里的做法。

## 工具

除了手动的方式修改代码外，我们推荐使用 [Grunt][5] 来进行统一的改造，官方也通过 Grunt 改造了很多主流模块和 jQuery 插件，具体的操作手册见 [引入 CMD 模块指南][6] 。你可以在 [cmdjs/gallery][2] 和 [cmdjs/jquery][4] 中找到具体的 Gruntfile ，从而借鉴到您的项目中去。所有打包好的模块可以在 [spmjs.org][7] 中找到。

## 小结

上面提供的是原有代码包装为 `CMD 书写规范` 的方法，在具体上线前，可能还需要打包为具名模块（包含 ID 的模块）。关于构建方面的进一步知识可以参考 [构建工具][8] 。

这里提到的包装方式都比较典型和简单，具体的实践可能千差万别。您在项目中有什么探索、实践和问题，欢迎来这里分享和提问。

   [1]: https://github.com/seajs/seajs/issues/242
   [2]: https://github.com/cmdjs/gallery
   [3]: https://github.com/cmdjs/gallery/blob/master/moment/Gruntfile.js
   [4]: https://github.com/cmdjs/jquery
   [5]: http://gruntjs.com
   [6]: https://github.com/cmdjs/gallery/blob/master/README.md
   [7]: https://spmjs.org/
   [8]: https://github.com/seajs/seajs/issues/538
  

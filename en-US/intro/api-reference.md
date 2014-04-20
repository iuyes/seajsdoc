---
name: API 快速参考
sort: 3
---

[Permalink](https://github.com/seajs/seajs/issues/266 "Permalink to API 快速参考 · Issue #266 · seajs/seajs · GitHub")

# API 快速参考 · Issue #266 · seajs/seajs · GitHub

该页面列举了 Sea.js 的常用 API。只要掌握这些用法，就可以娴熟地进行模块化开发。

* * *

### seajs.config

用来对 Sea.js 进行配置。


    seajs.config({

      // 设置路径，方便跨目录调用
      paths: {
        'arale': 'https://a.alipayobjects.com/arale',
        'jquery': 'https://a.alipayobjects.com/jquery'
      },

      // 设置别名，方便调用
      alias: {
        'class': 'arale/class/1.0.0/class',
        'jquery': 'jquery/jquery/1.10.1/jquery'
      }

    });


更多配置项请参考：[#262][1]

### seajs.use

用来在页面中加载一个或多个模块。


    // 加载一个模块
    seajs.use('./a');

    // 加载一个模块，在加载完成时，执行回调
    seajs.use('./a', function(a) {
      a.doSomething();
    });

    // 加载多个模块，在加载完成时，执行回调
    seajs.use(['./a', './b'], function(a, b) {
      a.doSomething();
      b.doSomething();
    });


更多用法请参考：[#260][2]

### define

用来定义模块。Sea.js 推崇一个模块一个文件，遵循统一的写法：


    define(function(require, exports, module) {

      // 模块代码

    });


也可以手动指定模块 id 和依赖，详情请参考：[#242][3]
`require`, `exports` 和 `module` 三个参数可酌情省略，具体用法如下。

### require

`require` 用来获取指定模块的接口。


    define(function(require) {

      // 获取模块 a 的接口
      var a = require('./a');

      // 调用模块 a 的方法
      a.doSomething();
    });


注意，`require` 只接受字符串直接量作为参数，详细约定请阅读：[#259][4]

### require.async

用来在模块内部异步加载一个或多个模块。


    define(function(require) {

      // 异步加载一个模块，在加载完成时，执行回调
      require.async('./b', function(b) {
        b.doSomething();
      });

      // 异步加载多个模块，在加载完成时，执行回调
      require.async(['./c', './d'], function(c, d) {
        c.doSomething();
        d.doSomething();
      });

    });


详细说明请参考：[#242][3]

### exports

用来在模块内部对外提供接口。


    define(function(require, exports) {

      // 对外提供 foo 属性
      exports.foo = 'bar';

      // 对外提供 doSomething 方法
      exports.doSomething = function() {};

    });


详细说明请参考：[#242][3]

### module.exports

与 `exports` 类似，用来在模块内部对外提供接口。


    define(function(require, exports, module) {

      // 对外提供接口
      module.exports = {
        name: 'a',
        doSomething: function() {};
      };

    });


`module.exports` 与 `exports` 的区别，以及详细说明请参考：[#242][3]

* * *

以上 7 个接口是最常用的，要牢记于心。

   [1]: https://github.com/seajs/seajs/issues/262 (配置)
   [2]: https://github.com/seajs/seajs/issues/260 (模块的加载启动)
   [3]: https://github.com/seajs/seajs/issues/242 (CMD 模块定义规范)
   [4]: https://github.com/seajs/seajs/issues/259 (require 书写约定)
  

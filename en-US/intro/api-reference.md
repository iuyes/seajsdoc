---
name: API Quick Reference
sort: 3
---

[Permalink](https://github.com/seajs/seajs/issues/266 "Permalink to API Quick Reference · Issue #266 · seajs/seajs · GitHub")

# API Quick Reference · Issue #266 · seajs/seajs · GitHub

We will discuss some common API in `Sea.js`. After learning them, you can do modular development smoothly.

* * *

### seajs.config

To set the configuration for `Sea.js`


    seajs.config({

      // Set up path for crossing folder usage.
      paths: {
        'arale': 'https://a.alipayobjects.com/arale',
        'jquery': 'https://a.alipayobjects.com/jquery'
      },

      // Set up alias for easily calling
      alias: {
        'class': 'arale/class/1.0.0/class',
        'jquery': 'jquery/jquery/1.10.1/jquery'
      }

    });


For more configuration options see [#262][1]

### seajs.use

To load one or more modules into the page.


    // Load a module
    seajs.use('./a');

    // Load a module and run callback after loading
    seajs.use('./a', function(a) {
      a.doSomething();
    });

    // Load more modules and run callbacks after loading
    seajs.use(['./a', './b'], function(a, b) {
      a.doSomething();
      b.doSomething();
    });


For more see [#260][1]

### define

To define a module. `Sea.js` recommend one-module-one-file standard:


    define(function(require, exports, module) {

      // Module implementation

    });


You can also set module id and dependencies. See more [#242][3]

`require`, `exports` and `module` can be ignored sometimes.

### require

Use `require` to get other module's interface.


    define(function(require) {

      // Get interface of module a
      var a = require('./a');

      // Call method of module a
      a.doSomething();
    });


NOTES: `require` only accept string value as the parameter. See more [#259][4]

### require.async

`require.async` used to load one or more modules inside a module asynchronously.


    define(function(require) {

      // Load a module asynchronously and run callback after loading
      require.async('./b', function(b) {
        b.doSomething();
      });

      // Load many modules asynchronously and run callback after loading
      require.async(['./c', './d'], function(c, d) {
        c.doSomething();
        d.doSomething();
      });

    });


See more: [#242][3]

### exports

Used to expose interface in the module.


    define(function(require, exports) {

      // Expose foo property
      exports.foo = 'bar';

      // Expose doSomething method
      exports.doSomething = function() {};

    });


See more [#242][3]

### module.exports

Similar as `exports`, used to expose interface in the module.


    define(function(require, exports, module) {

      // Expose interface
      module.exports = {
        name: 'a',
        doSomething: function() {};
      };

    });

The difference between `module.exports` and `exports` see [#242][3]

* * *

These 7 methods are most useful, keep them in mind.

   [1]: https://github.com/seajs/seajs/issues/262 (配置)
   [2]: https://github.com/seajs/seajs/issues/260 (模块的加载启动)
   [3]: https://github.com/seajs/seajs/issues/242 (CMD 模块定义规范)
   [4]: https://github.com/seajs/seajs/issues/259 (require 书写约定)
  

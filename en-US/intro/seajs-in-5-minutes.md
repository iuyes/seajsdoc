---
name: Learn SeaJS in 5 minutes
sort: 2
---


[Permalink](http://seajs.org/docs/ "Permalink to A Module Loader for the Web")

# A Module Loader for the Web

## Loading ...

## Easy but great experience of modular development.

A Module Loader for the Web

## Learn SeaJS in 5 minutes

Let's see how to build a mini game in 5 minutes.

### File structure

The source code is on GitHub: [seajs/examples][1]


    examples/
      |-- sea-modules      seajs, jquery and other libs. This is the module's deploy folder.
      |-- static           js and css files for every projects
      |     |-- hello
      |     |-- lucky
      |     `-- todo
      `-- app              html files
            |-- hello.html
            |-- lucky.html
            `-- todo.html


Let's start from `hello.html` to see how will `Sea.js` organize the code.

### Loading modules into page

At the bottom of `hello.html`, import `sea.js` by `script` tag and config SeaJS:


    // Configure seajs
    seajs.config({
      base: "../sea-modules/",
      alias: {
        "jquery": "jquery/jquery/1.10.1/jquery.js"
      }
    })

    // Load main module
    seajs.use("../static/hello/src/main")


After `sea.js` downloaded, it will load main module.

### Writing Module

This mini game has two module `spinning.js` and `main.js`:


    // Defining a module by `define`
    define(function(require, exports, module) {

      // Adding dependencies by `require`
      var $ = require('jquery');
      var Spinning = require('./spinning');

      // Exposing interfaces by `exports`
      exports.doSomething = ...

      // Providing all interfaces by `module.exports`
      module.exports = ...

    });


The the recommended CMD structure by `Sea.js`. If you know `Node.js` it will look so natural.

### Building and deploying

In real project, the code need to be compressed and combined before go live.
It can be done by `spm` or `Grunt`. See here [Building Tools][2]

### Summary

`Sea.js` can standardize the format of the modules, manage dependencies of the modules, organize the code structure, debug code and optimize the performance. I believe you will love it.

If you are interested, you can see more demos [seajs/examples][3]
If you've already loved it, you can keep on reading the [documents][4]

Ask questions and communicate [Community][5]

   [1]: https://github.com/seajs/examples
   [2]: https://github.com/seajs/seajs/issues/538
   [3]: http://seajs.github.io/examples
   [4]: http://seajs.org#docs
   [5]: https://github.com/seajs/seajs/issues/271

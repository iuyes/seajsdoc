---
name: Why do you need modular js structure for frontend development?
sort: 1
---

[Permalink](https://github.com/seajs/seajs/issues/547 "Permalink to 前端模块化开发的价值 · Issue #547 · seajs/seajs · GitHub")

# Why do you need modular js structure for frontend development? · Issue #547 · seajs/seajs · GitHub

This article is published at 《程序员》 2013, issue 3.

* * *

The frontend development is becoming more and more complicated. We will talk about how can modular js solve the problems in frontend development and reduce its complexity. We will also discuss how can we do modular front end development with Sea.js.

## Annoying Name Clashes.

In the real projects, we often use some common and utility functions such as:

    function each(arr) {
      // Implementation
    }

    function log(str) {
      // Implementation
    }


Usually we put them into util.js file and import this file when we need to use it. Colleagues also thank for the utilities.

But while the team growing someone start complaining.

>> Bob: I want to define my own `each` method. But it's already defined in utils.js, I can only give it another name such as `eachObject`.
>> 
>> Tom: I defined my own `log` function but why it doesn't work for Bob?

While more and more complains came, the team decided to import namespace to solve the Name Clashes issue as similar as Java.


    var org = {};
    org.CoolSite = {};
    org.CoolSite.Utils = {};

    org.CoolSite.Utils.each = function (arr) {
      // Implementation
    };

    org.CoolSite.Utils.log = function (str) {
      // Implementation
    };


This example is not made for this article. It has been used a lot in projects such as Yahoo's YUI2. The snippet below is from a opensource project of Yahoo.

    if (org.cometd.Utils.isString(response)) {
      return org.cometd.JSON.fromJSON(response);
    }
    if (org.cometd.Utils.isArray(response)) {
      return response;
    }


Through namespaces we can solve many name clashes issues. But you can't stop sympathizing when you see such long namespaces.

To solve this YUI introduced another mechanism.


    YUI().use('node', function (Y) {
      // Node module is loaded
      // Can be called by Y
      var foo = Y.one('#foo');
    });


YUI3 solved long namespaces issues by using sandbox mechanism. But it introduced some new issues.


    YUI().use('a', 'b', function (Y) {
      Y.foo();
      // Which module provided foo method, a or b?
      // If a and b both provide method a and b, how can we prevent name clashes?
    });


The name clashes is not as easy to solve as it looks. How can we solve it elegantly? Let's see another example.

## Complicated file dependenciest 

Continue with the story. Based on util.js we start developing UI common components so that other colleagues don't need to reinvent the wheels.

One of the popular component is dialog.js simply used like this:
    
    
      org.CoolSite.Dialog.init({ /* Configurations */ });
    
No matter how detailed the documents are, colleagues keep on asking questions about dialog.js. Finally the reason is found out that util.js is not imported before using dialog.js. So dislog.js can't work properly. This happens a lot in real project developing.

When project became complicated, the dependencies among files will drive you crazy. You face these problems a lot:

  1. Common component team upgraded base libs but it's hard to push the upgrade to the whole project.
  2. Bussiness team want to use a new common component but found out it can't be done easily.
  3. An old product need a new feature but it has to be developed base on the old libraries.
  4. Company merged businesses and products but the front end code are conflicting with each other.
  5. ……

Most of the problems are because of the file dependencies are not managed well. In frond end pages most of the file dependencies are guaranteed manually. It's fine when the team is small but while team growing and the bussiness logic becoming complicated, the dependency hell will become a big problem.

In most libraries such as YUI3 and KISSY, dependencies are managed by configurations.


    YUI.add('my-module', function (Y) {
      // ...
    }, '0.0.1', {
        requires: ['node', 'event']
    });


The snippet above use `requires` to declare the dependencies. It's enough in most cases but not elegant. When the application has many modules and complicated dependencies, tedious configuration may have many hidden issues.

Name clashes and file dependencies are two classical problems. Now let's see how can we solve them by modular js. We will use Sea.js as the exapmle.

## Using Sea.js

Sea.js is a powerful opensource project in order to be a easy modular development tool. 

Sea.js follows CMD (Common Module Definition). One file is one module. So the former util.js will become:


    define(function(require, exports) {
      exports.each = function (arr) {
        // Implementation
      };

      exports.log = function (str) {
        // Implementation
      };
    });


Useing `exports` to expose interface. So the former dialog.js will become:


    define(function(require, exports) {
      var util = require('./util.js');

      exports.init = function() {
        // Implementation
      };
    });


The key part is we used `require('./util.js')` to get the access to the interfaces exposed by `exports` in util.js. The `require` here you can consider it as a **keyword** SeaJS extended to JavaScript. You can get other modules's interfaces by `require`.

It's as similar as the `@import` in CSS:

    @import url("base.css");

    #id { ... }
    .class { ... }

The `require` in SeaJS give JavaScript code the ability to import dependenies:

It's also as similar as the `include`, `import` in other languages such as Java, Python C# and Go. The native importing feature of JavaScript is still in the draft and need to wait for the ES6 standard being supported by the common browsers.

Then it will become very easy to use dialog.js:

    
    seajs.use('dialog', function(Dialog) {
      Dialog.init(/* Configurations */);
    });
    

Firstly add `sea.js` in the web page. Then when you want to use some modular in the page, just use `seajs.use` to import it.

The two significant benefits `Sea.js` gave us are:

  1. **Exposing interfaces by `exports`**. It means we don't need namespaces anymore and certainly not global variables. It will solve the name clashes thoroughly.

  2. **Importing dependencies by `require`**. It means the developer only need to care about the dependencies of the current module. `Sea.js` will take care of other things. Now module developers can focus on the module and enjoy coding.

## Summary

Beside solve name clashes and dependencies control, using `Sea.js` for modular development has many other benefits:

  1. **Module's version control**. You can control the module's version easily by alias configuration and build tools.

  2. **Higher maintainability**. Modular structure is trying to make each file takes care of single task, it's very good for the code maintaince. `Sea.js` also provided such as `nocache` and `debug`. It can speed up the development.

  3. **Improving the front end performance**. `Sea.js` load modules asynchronously, it improves the performance. `Sea.js` also provide plugin such as `combo`, `flush` to cooperate with backend. It can optimize the performance easily.

  4. **Sharing modules crossing environment**. CMD module definition is similar as `Node.js`. It can sharing the modules crossing servers and browsers easily.

Modular development is not new but in Web front end development it is. It is raising with Dojo, YUI3, Node.js recently.

The front end modular development has two major categories. One category is the Cathedral model represented by Dojo, YUI3 and KISSY. In the Cathedral model every components are modular and they separated layer by layer and relayed on each other. Another category is the market model represented by jQuery, RequireJS, Sea.js and OzJS. In the market model, every components are independent from others, single function. Every component combines together loose and accomplish the task.

These two models have their own usage. 

In a word, modular development has many benefits. If you haven't tried it yet why not start from `Sea.js`.

* * *

### 2013-04-23

Here is a good introduction document [一步步学会使用 Sea.js 2.0][2]

   [1]: http://seajs.org/
   [2]: http://hi.baidu.com/liuda101/item/54bcf8d0b6a65602d68ed057
  

---
name: Module System
sort: 1
---


[Permalink](https://github.com/seajs/seajs/issues/240 "Permalink to 模块系统 · Issue #240 · seajs/seajs · GitHub")

# Module System · Issue #240 · seajs/seajs · GitHub

## What is system?

There are lots of systems in our life: ecosystem, operating system, office system, network system etc. But what is a system?

来看下维基百科的解释：
Let's see the definition in wikipedia:

&gt; A system is a set of interacting or interdependent components forming an integrated whole or a set of elements (often called 'components' ) and relationships which are different from relationships of the set or its elements to other elements or sets.


简言之，系统有两个基本特性：
In short, system has two features:

  1. Formed by components.
  2. There are relationships between components and they fulfill the function or purpose together.

To form a system at least two factor need to be set:

  1. **define components**: decide what's the components.
  2. **set up relationships**: decide how can the components communicate and what's the rule.

If we can figure out these two things, we can form a system.

## Module System

`Sea.js` is a module loader for Web browser. In `Sea.js`, everything is module, all the modules formed to a module system. `Sea.js` need to solve the system's two factors:

  1. What's the module?
  2. How can the module communicate?

In front end development territory, a module can be a JS module, a CSS module or a Template module. In `Seaj.js`, we focus on JS module (other module can convert to JS module):

  1. A module is a piece of JavaScript snippet with specific structure.
  2. Modules can interact with each other and working together.

If you can correctly define the model structure and communication rule, you can form a model system. The model structure and the communication rule are Module Definition Specification, such as CommonJS's [Modules 1.1.1][1], NodeJS's [Modules][2], and RequireJS's [AMD][3] etc.

`Sea.js` is following [CMD][4].

## Further reading

   [1]: http://wiki.commonjs.org/wiki/Modules
   [2]: http://nodejs.org/api/modules.html
   [3]: https://github.com/amdjs/amdjs-api/wiki/AMD
   [4]: https://github.com/cmdjs/specification/blob/master/draft/module.md
  

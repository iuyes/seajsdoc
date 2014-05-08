---
name: Standard building
sort: 7
---

[Permalink](https://github.com/seajs/seajs/issues/538 "Permalink to 构建工具 · Issue #538 · seajs/seajs · GitHub")

## Standard building

If the project follows the standard structure:

    
    foo-module/
      |-- dist                    For the builded files
      |-- src                     For the source code of js and css
      |     |-- foo.js
      |     `-- style.css
      `-- package.json      module information
    

then the building is very easy. Firstly, install `spm` tool:

    
    $ npm install spm -g
    $ npm install spm-build -g
    

then run building command:

    
    $ cd foo-module
    $ spm build
    

It will build source code to `dist` folder based on `package.json`. After building we need to deploy files in `dist` to `sea-modules` folder, such as `make deploy` command [Makefile](https://github.com/seajs/examples/blob/master/static/hello/Makefile) in examples.

See more [http://docs.spmjs.org/](http://docs.spmjs.org/)

You can clone and try [seajs/examples](https://github.com/seajs/examples) locally.

## Customized Building

If standard building is not enough, you can use Grunt. [http://gruntjs.com/](http://gruntjs.com/)

Using Grunt to build CMD module need these Grunt tasks:

  * [grunt-cmd-transport](https://github.com/spmjs/grunt-cmd-transport)
  * [grunt-cmd-concat](https://github.com/spmjs/grunt-cmd-concat)

This part is very easy. Just get familar with Grunt first.

Recommended articles:

  * [CMD 模块构建，从认识 Grunt 开始](https://github.com/seajs/seajs/issues/670)
  * [如何使用 Grunt 构建一个中型项目](https://github.com/seajs/seajs/issues/672)
  * [使用 spm 构建业务模块的个人经验](https://github.com/seajs/seajs/issues/690)

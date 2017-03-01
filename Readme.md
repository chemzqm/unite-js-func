# Unite-js-func

A plugin of [unite.vim](https://github.com/Shougo/unite.vim) helps to find javascript functions quickly.

**This plugin is deprecated, consider use [denite-extra](https://github.com/chemzqm/denite-extra) instead**

## Features

* Find function in current file
* Find function of related files (required by `require` function)
* Find function of current module, one specified module, all dependency modules
* Support module alias (`browser` field in `package.json`)

## Install

A npm module need to be installed globally, install [node](https://nodejs.org)
and run:

    npm install -g parsefunc

Install `unite`, `vimproc` and `units-js-func`, for example with [vundle](https://github.com/gmarik/vundle), you can add:

    Plugin 'chemzqm/unite-js-func'
    " unite is requried
    Plugin 'Shougo/unite.vim'
    " vimproc is required by unite
    Plugin 'Shougo/vimproc'

to your `.vimrc` and run:

    :so ~/.vimrc
    :BundleInstall

## Usage

* Find function of current file

      :Unite func

* Find function of related files (exclude ralated modules)

      :Unite func:e

* Find function of a specified related module

      :Unite func:m:module_name

* Find function of current module

      :Unite func:t

* FInd function of all related modules

      :Unite func:m

_My vim configuration for this plugin https://gist.github.com/chemzqm/7501f2ba0ccab3e5030c#file-vimrc-vim_

## Thanks

* [Shougo/unite](https://github.com/Shougo/unite.vim) interface for search and display information from arbitrary sources.
* [acorn](https://github.com/ternjs/acorn) parse javascript AST.
* [webpack/enhanced-resolve](https://github.com/webpack/enhanced-resolve) resolve javascript module dependencies.

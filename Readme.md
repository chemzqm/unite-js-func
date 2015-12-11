# Unite-js-func

快速定位 javascript 函数的 [unite.vim](https://github.com/Shougo/unite.vim) 插件。

支持多种查找模式，支持模块别名（通过设置 package.json 中的 browser 属性），

暂时还不支持 es2015，过段时间会完成

![func](https://cloud.githubusercontent.com/assets/251450/11688504/070ed288-9ec8-11e5-8b99-3408e8b9cb50.gif)


## 安装

需要安装 [parsefunc](https://github.com/chemzqm/parsefunc) 来完成解析功能，它是一个命令行工具，通过 npm 安装：

    npm install -g parsefunc

然后使用你熟悉的 vim 包工具进行安装，例如：[vundle](https://github.com/gmarik/vundle)

.vimrc 中添加：

    Plugin 'chemzqm/unite-js-func'
    " unite 也是必须的
    Plugin 'Shougo/unite.vim'
    " vimproc 可完成异步操作，可选
    Plugin 'Shougo/vimproc'

然后安装：

    :so ~/.vimrc
    :BundleInstall


## 使用

* 查找当前文件的函数

      :Unite func

* 查找当前文件非模块引用的函数（通过相对路径 require 的文件）

      :Unite -custom-func-type=r

* 查找当前文件模块引用的全部函数（通过模块名进行 require）

      :Unite -custom-func-type=e

* 查找指定模块的全部函数 (支持 webpack browsersify npmjs 所支持的 [browser 别名](https://gist.github.com/defunctzombie/4339901) )

      :Unite -custom-func-type=m -custom-func-name=moduleName

* 查找当前模块内的全部函数 (通过递归解析 package.json 中 main 定义的主文件的依赖)

      :Unite -custom-func-type=t

* 查找所有依赖模块（package.json 中由 dependencies 定义）的全部函数

      :Unite -custom-func-type=m

## 使用自定义命令查找

可参考我的 vim 配置：https://gist.github.com/chemzqm/7501f2ba0ccab3e5030c

## 小技巧

这个插件实现了 unite 的 jump_list, 就像 `unite grep` 一样，你可以通过多种方式与列表进行交互， 例如：

* 使用回车在当前面窗口内打开文件
* 使用 `t` 在新标签页打开文件
* 使用 `p` 打开预览窗口
* 使用 `i` 进入插入模式输入更多关键字
* 使用 `q` 关闭列表以及预览窗口（如果存在）

## 特别感谢

* [Shougo/unite](https://github.com/Shougo/unite.vim) 强大且灵活的操作界面，成功取代了我很多插件(例如 nerdtree, ctrlp)
* [ternjs/acorn](https://github.com/ternjs/acorn) 解析 javascript AST
* [webpack/enhanced-resolve](https://github.com/webpack/enhanced-resolve) 灵活的依赖解析

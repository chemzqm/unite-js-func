# Unite-js-func

快速定位 javascript 函数的 [unite.vim](https://github.com/Shougo/unite.vim) 插件。

支持多种查找模式，支持模块别名（通过设置 package.json 中的 browser 属性），

暂时还不支持 es2015，过段时间会完成

![func](https://cloud.githubusercontent.com/assets/251450/11688504/070ed288-9ec8-11e5-8b99-3408e8b9cb50.gif)


## 安装

需要安装 [parsefunc](https://github.com/chemzqm/parsefunc) 来完成解析功能，它是一个命令行工具，通过 npm 安装：

    npm install -g parsefunc

然后使用你熟悉的包工具进行安装，例如：[vundle](https://github.com/gmarik/vundle)

.vimrc 中添加：

    Plugin 'chemzqm/unite-js-func'

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

通过自定义命令可以更便捷的查找函数，以下是我在 `.vimrc` 中的配置:

``` VimL
autocmd FileType javascript :call SetLoadFunctions()

function! SetLoadFunctions()
  " 当前文件
  command! -nargs=? -bar -buffer F  execute 'call LoadFunctions("c", "<args>")'
  " 当前模块
  command! -nargs=? -bar -buffer Ft execute 'call LoadFunctions("t", "<args>")'
  " 相对引用文件
  command! -nargs=? -bar -buffer Fr execute 'call LoadFunctions("r", "<args>")'
  " 全部模块
  command! -nargs=? -bar -buffer Fa execute 'call LoadFunctions("a", "<args>")'
  " 模块引用
  command! -nargs=? -bar -buffer Fe execute 'call LoadFunctions("e", "<args>")'
  " 指定模块
  command! -nargs=+ -bar -buffer Fm execute 'call LoadFunctions("m", "<args>")'
endfunction

function! LoadFunctions(type, args)
  let args = split(a:args, ' ')
  if !len(args)| let args = [''] |endif
  let input = args[0]
  let type = a:type
  let name =''
  if a:type ==# 'a'
    let type = 'm'
  endif
  if a:type ==# 'm'
    let type = "m"
    let name = args[0]
    let input = len(args) > 1 ? args[1] : ''
  endif
  if len(name)
    execute 'Unite func -custom-func-type=' . type . ' -custom-func-name=' . name . ' -input=' . input
  else
    execute 'Unite func -custom-func-type=' . type . ' -input=' . input
  endif
endfunction
```

这些配置实现了通过 `:F(a/t/r/e/m) name` 快速查找函数的功能

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

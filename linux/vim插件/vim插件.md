# vim插件安装及我的安装配置

在远程编辑服务器上的文件时，vim真的是超级好用。如果安装了一些插件之后会更好用，网上有很多教程把vim设置到很花哨。这里我只记录一下怎么安装vim插件，以及我目前在用的插件。

### vim插件的安装
我采用的vim插件管理器是[Vundle](https://github.com/VundleVim/Vundle.vim)。具体如何安装参考的下面两个帖子：
[vim插件管理器：Vundle的介绍及安装（很全）](https://blog.csdn.net/zhangpower1993/article/details/52184581)
[Vim插件管理器Pathogen和Vundle简介 - Vim入门教程(8)](https://vimjc.com/vim-plugin-manager.html)

### vim插件配置
接下来就是选择需要的插件，并进行配置，即配置`.vimrc`文件。我的配置文件是这样的:

[下载文件](./vimrc)

```
set nocompatible               "去除VIM一致性，必须"
filetype off                   "必须"

"设置包括vundle和初始化相关的运行时路径"
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
"
""启用vundle管理插件，必须"
Plugin 'VundleVim/Vundle.vim'

"在此增加其他插件，安装的插件需要放在vundle#begin和vundle#end之间"
""安装github上的插件格式为 Plugin '用户名/插件仓库名'"
Plugin 'vim-airline/vim-airline'
let g:airline_powerline_fonts = 1

Plugin 'yinflying/matlab.vim'
autocmd BufEnter *.m    compiler mlint

Plugin 'scrooloose/nerdtree'
autocmd VimEnter * NERDTree
wincmd w
autocmd VimEnter * wincmd w
autocmd bufenter * if (winnr("$") == 1 && exists("b:NERDTree") && b:NERDTree.isTabTree()) | q | endif

Plugin 'scrooloose/syntastic'
let g:syntastic_error_symbol='>>'
let g:syntastic_warning_symbol='>'
let g:syntastic_check_on_open=1
let g:syntastic_check_on_wq=0
let g:syntastic_enable_highlighting=1
let g:syntastic_python_checkers=['pyflakes'] " 使用pyflakes,速度比pylint快
let g:syntastic_javascript_checkers = ['jsl', 'jshint']
let g:syntastic_html_checkers=['tidy', 'jshint']
" 修改高亮的背景色, 适应主题
highlight SyntasticErrorSign guifg=white guibg=black

" to see error location list
let g:syntastic_always_populate_loc_list = 0
let g:syntastic_auto_loc_list = 0
let g:syntastic_loc_list_height = 5
function! ToggleErrors()
    let old_last_winnr = winnr('$')
    lclose
    if old_last_winnr == winnr('$')
        " Nothing was closed, open syntastic error location panel
        Errors
    endif
endfunction
nnoremap <Leader>s :call ToggleErrors()<cr>
" nnoremap <Leader>sn :lnext<cr>
" nnoremap <Leader>sp :lprevious<cr>


call vundle#end()              
filetype plugin indent on      "加载vim自带和插件相应的语法和文件类型相关脚本，必须"

"Added by my self"
set number
```

其中：
- `nerdtree`是我觉得最有用的插件，并做了一些配置保证用的很顺手；
- `matlab`的插件可以显示matlab的语法，而且双击`=`可以调整格式，也很好用。
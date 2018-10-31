+++ 
draft = false
date = 2014-07-04T18:06:00-04:00
title = "Setup VIM for Go development"
slug = "" 
tags = []
categories = ["Go"]
thumbnail = "<no value>"
description = ""
+++

This post is for someone like me who just get started with VIM, and because the original plugin author doesn't given detailed steps about how to setup his awesome work, here I am.

**NOTICE** This is not a post about how to install Go.

VIM plugin we use: [github.com/fatih/vim-go](https://github.com/fatih/vim-go)

### Install Pathogen

Pathogen is a plugin manager of VIM, which tons of plugins support it. So, one time forever, let's install it.

1. Go to [Pathogen home page](http://www.vim.org/scripts/script.php?script_id=2332), find the ZIP archive of package.
2. Download and unzip, you'll have a directory called `autoload` put the file named `pathogen.vim` to `~/.vim/autoload/pathogen.vim`.
3. Edit `~/.vimrc` file and add `call pathogen#infect()` to the top.

### Install VIM-GO

Now, let's install plugins we want.

1. Get into directory `~/.vim/bundle` then execute `git clone https://github.com/fatih/vim-go.git`.
2. Edit `~/.vimrc` file and add following content(the last line is for not auto-installing missing packages, which may need long time):

```
syntax enable
filetype plugin on
set number
let g:go_disable_autoinstall = 0

" Highlight
let g:go_highlight_functions = 1
let g:go_highlight_methods = 1
let g:go_highlight_structs = 1
let g:go_highlight_operators = 1
let g:go_highlight_build_constraints = 1
```

3. Great, now you can follow some instructions on [github.com/fatih/vim-go](https://github.com/fatih/vim-go) to custom. Note that use `<C-x><C-o>` call code completion, and better call it after you type `.`.

### Install neocomplete

This is the plugin gives you real-time code completion, but it has special requirements for your VIM, see  [github.com/Shougo/neocomplete.vim](https://github.com/Shougo/neocomplete.vim) for more details.

1. Get into directory `~/.vim/bundle` and execute `git clone https://github.com/Shougo/neocomplete.vim.git`.
2. Edit `~/.vimrc` file and add line `let g:neocomplete#enable_at_startup = 1`. Now the real-time feature will be enabled automatically every time you start your VIM.

### Install molokai theme

The same author wrote a molokai theme for VIM: [github.com/fatih/molokai](https://github.com/fatih/molokai).

To install it, just download his `molokai.vim` and put it into `~/.vim/colors` . Then add `colorscheme molokai` to your `~/.vimrc` file.

![](/img/140704/201407020359034.png)

### Install tagbar

This one is optional, but I installed it just because it looks so cool!

1. First of all, you have to install `ctags`, I use Mac so just simply `brew install ctags`.
2. Then execute `go get -u github.com/jstemmer/gotags` to install Go parser.
3. Add following lines to your `~/.vimrc` file:

```
let g:tagbar_type_go = {
    \ 'ctagstype' : 'go',
    \ 'kinds'     : [
        \ 'p:package',
        \ 'i:imports:1',
        \ 'c:constants',
        \ 'v:variables',
        \ 't:types',
        \ 'n:interfaces',
        \ 'w:fields',
        \ 'e:embedded',
        \ 'm:methods',
        \ 'r:constructor',
        \ 'f:functions'
    \ ],
    \ 'sro' : '.',
    \ 'kind2scope' : {
        \ 't' : 'ctype',
        \ 'n' : 'ntype'
    \ },
    \ 'scope2kind' : {
        \ 'ctype' : 't',
        \ 'ntype' : 'n'
    \ },
    \ 'ctagsbin'  : 'gotags',
    \ 'ctagsargs' : '-sort -silent'
\ }
```

4. Just like install other plugins, get into `~/.vim/bundle` and execute `git clone https://github.com/majutsushi/tagbar.git`.
5. Edit `~/.vimrc` file and add line `nmap <F8> :TagbarToggle<CR>`. This is a key mapping, which allows you use `F8` to enable/disable the feature.

![](/img/140704/201407020405489.png)

### Install file browser nerdtree

1. Get into `~/.vim/bundle` and execute `git clone https://github.com/scrooloose/nerdtree.git`.
2. Edit `~/.vimrc` and add line `map <C-n> :NERDTreeToggle<CR>`. Now you can press `<Ctrl+n>` to enable/disable this feature.

Awesome work!

![](/img/140704/201407030213312.png)

## Summary

Please read VIM-GO's documentation for more advanced usage, this post only can help you get started.

Good good study, day day up!
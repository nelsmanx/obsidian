## Permanent Line Numbering

In Vim configuration file `~/.vimrc` add `set number
___

## Exit visual mode without delay (eliminating delays on ESC)

In Vim configuration file `~/.vimrc` add `set ttimeoutlen=0`
___

### Color scheme (vim-plug)

1. Install [vim-plug](https://github.com/junegunn/vim-plug)
```bash 
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

2. Add [onedark.vim](https://github.com/joshdick/onedark.vim)  
```
# ~/.vimrc

call plug#begin()
Plug 'joshdick/onedark.vim'
call plug#end()

syntax on
colorscheme onedark
```

or [catppuccin.vim](https://github.com/catppuccin/vim)
```
call plug#begin()
" Plug 'joshdick/onedark.vim'
Plug 'catppuccin/vim', { 'as': 'catppuccin' }
call plug#end()

syntax on
set termguicolors
colorscheme catppuccin_macchiato
```

3. Install plugins. 
Reload the file or restart Vim, then `:PlugInstall` to install the plugins.
`:PlugClean` removes plugins no longer in the list.
___

## Status line (lightline)

1. Install [lightline.vim](https://github.com/itchyny/lightline.vim) (like vim-plug above)
```
# ~/.vimrc

call plug#begin()
Plug 'itchyny/lightline.vim'
call plug#end()

set laststatus=2 "always show statusline
set noshowmode   "hide modes in command line (inser, visual etc)
set shortmess+=F "don't show info (file" 14L, 272B) in command line on start
let g:lightline = {'colorscheme': 'catppuccin_frappe',}
```
___







## Keyboard Shortcuts

### Motions

`{number}G` - to the line number
`gj`, `gk` - down, up in multiline (line, that occupies more then one)
`gg`, 'G' - to first, last line



## Actions

`db` | `ctrl + W` -  delete the word to the left of the cursor


## Other

`u`, `ctrl + R` - undo, redo
`.` - repeat last operation
`zz` - scroll current line to center
k





### Exiting

 `:wq` | `:x` | `ZZ` - write (save) and quit
 `:q` - quit  (fails if there are unsaved changes)
 `:q!` |  `ZQ` - quit without saving
# Chapter 7: The .vimrc (Making Peace with the Beast)

*"My .vimrc is 500 lines of configuration to make Vim behave like a normal editor. I could have just used a normal editor."*  
— Reformed Vim User, 2021

## The Configuration File That Rules Them All

In Emacs, we have `~/.emacs.d/init.el`. It's Elisp. It's programmable. It's powerful. It makes sense.

In Vim, we have `~/.vimrc`. It's written in Vimscript, a language that exists solely to configure Vim, like some kind of recursive nightmare. It's as if someone said, "Let's create a programming language, but make it weird and only useful for one thing."

## Where Does It Live?

- Linux/Mac: `~/.vimrc` or `~/.vim/vimrc`
- Windows: `~/_vimrc` or `~/vimfiles/vimrc`
- Neovim (because Vim needed a fork): `~/.config/nvim/init.vim` or `~/.config/nvim/init.lua`

Yes, even the location is inconsistent. Because Vim.

## The Absolute Minimum .vimrc (Making Vim Bearable)

Here's the bare minimum to make Vim slightly less hostile:

```vim
" The Survival Kit
set nocompatible          " Be iMproved, not vi (yes, this needs to be said)
syntax on                 " Syntax highlighting (revolutionary!)
set number               " Show line numbers (like every editor since 1990)
set relativenumber       " Show relative line numbers (for the hjkl addicts)
set ruler                " Show cursor position (bottom right)
set showcmd              " Show partial commands (bottom right)
set wildmenu             " Visual autocomplete for commands
set showmatch            " Highlight matching [{()}]
set incsearch            " Search as you type (like Emacs!)
set hlsearch             " Highlight search results (annoyingly)

" Make it slightly less painful
set expandtab            " Tabs are spaces (join the right side)
set tabstop=4            " Number of visual spaces per TAB
set shiftwidth=4         " Number of spaces for indent
set autoindent           " Auto-indent new lines
set smartindent          " Smart indent (sometimes)

" Because ESC is too far
inoremap jj <Esc>        " Type 'jj' to escape INSERT mode
inoremap jk <Esc>        " Or 'jk' if you prefer

" Save my fingers
nnoremap ; :             " Use ; instead of : for commands
vnoremap ; :             " Same in visual mode
```

## The Settings Archaeology (Options From the Dawn of Time)

### The Compatible/Nocompatible Saga

```vim
set nocompatible
```

This tells Vim to not be compatible with vi. Vi is from 1976. This setting exists because somewhere, someone might want their 2024 editor to behave like it's 1976. It's like having a setting in your car to "enable horse-and-buggy mode."

### The Tab Wars Configuration

```vim
set expandtab            " Use spaces instead of tabs
set tabstop=4            " Display tabs as 4 spaces
set softtabstop=4        " Soft tab stop (whatever that means)
set shiftwidth=4         " Indent with 4 spaces
set autoindent           " Copy indent from current line
set smartindent          " Smart autoindenting (sometimes works)
set cindent              " C-style indenting (for C programmers)
```

Seven different settings for tabs and indentation. In Emacs? `(setq-default indent-tabs-mode nil)` and `(setq-default tab-width 4)`. Done.

### The Search Settings

```vim
set ignorecase           " Case insensitive search
set smartcase            " Unless you type capitals
set incsearch            " Incremental search
set hlsearch             " Highlight matches
set magic                " Regular expressions are magic (literally)
```

That `magic` setting? It changes how regex works. Because having four different regex modes wasn't enough.

## Key Remapping (Fixing Vim's Mistakes)

### The Leader Key (Vim's Prefix Key)

```vim
let mapleader = ","      " Set leader to comma (default is \)

" Now you can use leader for custom commands
nnoremap <leader>w :w<CR>    " ,w to save
nnoremap <leader>q :q<CR>    " ,q to quit
```

It's like Emacs's `C-x` prefix, but you have to define it yourself.

### The Great Escape Remapping

Everyone remaps escape because reaching for ESC is painful:

```vim
" Popular escape remappings (pick your poison)
inoremap jj <Esc>
inoremap jk <Esc>
inoremap kj <Esc>
inoremap ii <Esc>
inoremap <C-c> <Esc>
```

The fact that everyone remaps ESC tells you everything about Vim's design.

### Arrow Keys (The Controversy)

```vim
" Disable arrow keys (for the hardcore)
noremap <Up> <NOP>
noremap <Down> <NOP>
noremap <Left> <NOP>
noremap <Right> <NOP>
```

Some people disable arrow keys to force themselves to use hjkl. These are the same people who remove the ladder from the pool in The Sims.

## Plugin Management (Because Stock Vim Is Suffering)

### The Plugin Managers (Yes, Plural)

Vim doesn't have a built-in package manager. Instead, we have:
- Pathogen (the old way)
- Vundle (the older new way)
- Vim-Plug (the newer new way)
- Packer (for Neovim)
- Lazy.nvim (the newest new way)

Here's Vim-Plug because it's currently popular (this week):

```vim
" Install vim-plug if not installed
if empty(glob('~/.vim/autoload/plug.vim'))
  silent !curl -fLo ~/.vim/autoload/plug.vim --create-dirs
    \ https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
endif

" Plugins
call plug#begin('~/.vim/plugged')
Plug 'tpope/vim-sensible'        " Sensible defaults (admission of guilt)
Plug 'tpope/vim-fugitive'        " Git integration
Plug 'preservim/nerdtree'        " File tree (because netrw is awful)
Plug 'vim-airline/vim-airline'   " Status bar (because default is from 1991)
Plug 'morhetz/gruvbox'          " Color scheme (because default hurts)
call plug#end()
```

Notice that first plugin? "vim-sensible"? It's literally a plugin to make Vim have sensible defaults. The fact this exists and is popular is Vim's greatest self-own.

## Color Schemes (Making It Pretty Won't Fix the Pain)

```vim
" Enable 256 colors (welcome to 1987!)
set t_Co=256

" True color support (if your terminal is from this century)
if has('termguicolors')
  set termguicolors
endif

" Set color scheme
colorscheme gruvbox
set background=dark
```

Default Vim colors look like someone ate a box of crayons and threw up on your terminal. You need a color scheme just to make text readable.

## The Actually Useful Settings

Here are configurations that genuinely improve Vim:

```vim
" Persistent undo (survives closing Vim)
set undofile
set undodir=~/.vim/undodir

" Better backspace behavior
set backspace=indent,eol,start

" Show whitespace characters
set list
set listchars=tab:›\ ,trail:•,extends:#,nbsp:.

" Better split behavior
set splitbelow
set splitright

" Faster updates
set updatetime=100

" Don't redraw during macros
set lazyredraw

" Mouse support (yes, mouse!)
set mouse=a
```

That last one—mouse support—is heresy to Vim purists. But you know what? Sometimes you just want to click where you want to edit. Revolutionary.

## The Copy-Paste Configuration (The Eternal Struggle)

```vim
" Use system clipboard
set clipboard=unnamedplus  " Linux
" set clipboard=unnamed     " Mac/Windows

" Better paste behavior
set paste                   " But this breaks autoindent
set nopaste                 " So you toggle it constantly
```

Getting copy-paste to work between Vim and the system is like performing a satanic ritual. Sometimes it works, sometimes it doesn't, and you're never quite sure why.

## Auto Commands (Making Vim Do Things Automatically)

```vim
" Remove trailing whitespace on save
autocmd BufWritePre * %s/\s\+$//e

" Return to last edit position
autocmd BufReadPost * if line("'\"") > 0 && line("'\"") <= line("$")
  \| exe "normal! g'\"" | endif

" Different settings for different file types
autocmd FileType python setlocal tabstop=4
autocmd FileType javascript setlocal tabstop=2
autocmd FileType yaml setlocal tabstop=2
```

Autocmds are powerful but dangerous. One wrong autocmd and Vim does mysterious things forever.

## The Status Line (Information Overload)

```vim
" Custom status line (before giving up and using airline)
set statusline=%F                           " Full path
set statusline+=%m                          " Modified flag
set statusline+=%r                          " Readonly flag
set statusline+=%h                          " Help file flag
set statusline+=%=                          " Switch to right side
set statusline+=%{&fileencoding?&fileencoding:&encoding}
set statusline+=\ [%{&fileformat}]         " File format
set statusline+=\ %p%%                      " Percentage
set statusline+=\ %l:%c                     " Line:Column
set statusline+=\ 
```

Or just install airline/lightline and call it a day.

## A Real .vimrc Evolution

**Day 1**: "I'll just use defaults"  
**Day 7**: "Maybe line numbers would be nice"  
**Day 30**: "100 lines of config"  
**Day 365**: "500 lines, 30 plugins"  
**Day 366**: "Switches to Neovim"  
**Day 400**: "Lua configuration"  
**Day 500**: "Installs Emacs"

## The .vimrc Maintenance Cycle

1. Find annoying Vim behavior
2. Google "vim disable [annoying thing]"
3. Add configuration line
4. Realize it breaks something else
5. Add more configuration to fix that
6. Repeat until .vimrc is 1000 lines
7. Declare bankruptcy, start over
8. Repeat

## Common .vimrc Mistakes

```vim
" Forgot the quotes (this is a comment)
set number              This breaks everything

" Wrong map mode
nmap <leader>w :w<CR>   " Works in normal mode
imap <leader>w :w<CR>   " Types ":w<CR>" in your document

" Recursive mapping hell
map j gj                " j now does gj
map gj j                " Infinite loop!
```

## The Ultimate .vimrc

```vim
" The honest .vimrc
set nocompatible
echo "Why are you doing this to yourself?"
echo "Emacs is right there."
echo "It has normal keybindings."
echo "You can type without switching modes."
echo "Copy-paste just works."
echo ""
echo "But no, here you are."
echo "Configuring Vim."
echo "Again."
```

## Next Up

Now that you've configured Vim to be slightly less hostile, let's learn the survival commands you'll actually need when you're stuck on a server with only Vim available.

---

*Next Chapter: [Survival Commands for Server Life](chapter-08-survival.md)*

*Remember: Every line in your .vimrc is an admission that Vim's defaults are wrong. Emacs users have init.el files too, but they're adding features, not fixing fundamental usability issues. There's a difference.*
# Vim

In a bid to increase productivity, I've been trying to limit my use of the track pad, and learning VIM is one of the things that have been consistently recommended.

Vim \(Vi IMproved\) is a clone of the text editor Vi, with additional functionalities. It was released November 1991, and is one of the more popular editors, even today.

It's highly customizable and includes the ability to create macros with the aid of vim scripts. 

You can also install extensions that provide emulate vim functionalities in the browser \(Vimium\) or your IDE \(VSCodeVim, etc\)

## Quickstart

`$vimtutor` is probably already available on your linux terminal. If not, you can install the vim editor.

[Open vim](https://www.openvim.com/) is a good starting online interactive vim tutorial.

[Vim Adventures](https://vim-adventures.com/) is a fun online game to learn vim. \(But the free version only has a few levels.\) 

[Vim cheatsheet](https://vim.rtorr.com/)

## Basic vim commands:

`:help [keyword]`

`:e [file]` open a file

`:w` \(write\) save the current file

`:w [filename]` save as filename

`:wq` save and quit

`:q!` quit without saving

## Movements:

`h` left

`l` right

`j` down

`k` up

`H` top of the screen

`M` middle of the screen

`L` bottom of the screen

`w` start of next word, `b` start of previous word

`e` end of the next word

`0` beginning of the line, `$` end of the line

`(` start of previous sentence, `)` start of next sentence

`{` start of previous paragraph, `}` start of next paragraph

`Ctrl+f` one page forward

`Ctrl+b` one page backwards

`gg` start of file

`G` end of file

`# [number]` go to line \[number\]

## Editing:

`v` visual mode to select characters

`y[motion]` copy \(yank\), where motion denotes vim motions

`d[motion]` delete \(cut\)

`p` paste

`yy` copy a line

`y$`  copy from where your cursor is to the end of a line

`dd` delete a line of text

`D` delete from where your cursor is to the end of a line

`x` delete a single character

`u` undo, `r` redo

`.` repeat last action

## Searching:

`/[keyword]` search for keyword

`n` next

`N` previous

`:%s/[pattern]/[replacement]/g` - replace all occurrences of a pattern without confirmation

`:%s/[pattern]/[replacement]/gc` - replace all occurrences of a pattern and confirms each one

## Navigating files:

`:bn` switch to the next buffer, `:bp` switch to previous buffer

`:bd` close a buffer

`:sp [filename]` open new file and splits screen horizontally

`:vsp [filename]` open new file and splits screen vertically

`:ls` list all open buffers

`Ctrl+ws` split windows horizontally

`Ctrl+wv` split windows vertically

`Ctrl+ww` switch between windows

`Ctrl+wq` quit a window

`Ctrl+wh` move cursor to the window to the left

`Ctrl+wl` move cursor to the window to the right

`Ctrl+wj` move cursor to the window downwards

`Ctrl+wk` move cursor to the window upwards

## Tab pages:

`:tabedit [file]` open a new tab to edit file

`gt` move to next tab, `gT` move to previous tab

`#gt` go to specific tab, e.g. `2gt` goes to the second tab




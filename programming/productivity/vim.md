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

`:e [file]` opens a file

`:w` \(write\) saves the current file

`:w [filename]` saves as filename

`:wq` saves and quit

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

`yy` copies a line

`y$`  copies from where your cursor is to the end of a line

`dd` deletes a line of text

`D` deletes from where your cursor is to the end of a line

`x` deletes a single character

`u` undo, `r` redo

`.` repeats last action

## Searching:

`/[keyword]` search for keyword

`n` next

`N` previous

`:%s/[pattern]/[replacement]/g` - This replaces all occurrences of a pattern without confirming each one

`:%s/[pattern]/[replacement]/gc` - Replaces all occurrences of a pattern and confirms each one

## Navigating files:

`:bn` switch to the next buffer, `:bp` switch to previous buffer

`:bd` close a buffer

`:sp [filename]` opens new file and splits screen horizontally

`:vsp [filename]` opens new file and splits screen vertically

`:ls` lists all open buffers

`Ctrl+ws` split windows horizontally

`Ctrl+wv` split windows vertically

`Ctrl+ww` switch between windows

`Ctrl+wq` quit a window

`Ctrl+wh` move cursor to the window to the left

`Ctrl+wl` move cursor to the window to the right

`Ctrl+wj` move cursor to the window downwards

`Ctrl+wk` move cursor to the window upwards

## Tab pages:






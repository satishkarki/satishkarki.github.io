---
layout: post
title: "Mastering Vim"
date: 2021-05-09 22:00:00
categories: Text_Editor
tags: text_editior linux # TAG names should be lowercase
image:
    path: /assets/img/headers/VIM.webp
---
So, I thought to my self, why not start a blog. A blog about the things, I love doing. The first thing I needed was a good text editor. Dennis Simpson and his [courses](https://zonzorp.github.io/) about Information System Security introduced me to the Linux world. The guy was a wizard with Vim. I wished to be like him. Here I am now, with [The Ultimate Markdown Cheat Sheet](https://towardsdatascience.com/the-ultimate-markdown-cheat-sheet-3d3976b31a0) and the book **Mastering Vim** by Ruslan Osipov hoping one day I will be like Dennis.

***
## Vim
A modal interface, where each trigger performs a different action based upon context. 

## Setting up on Linux
```bash
git clone https://github.com/vim/vim.git
cd vim/src
make
sudo make install
```
We can update Vim using a package manger as well.
```bash
sudo apt-get update
sudo apt-get install vim-gtk
```
We can start the editor by typing `vim` or `vi`. On older systems the two are different binaries. There are graphical version of Vim like Vanilla Vim and GVim. 
## Verifying installation and trouble shooting
```bash
vim --version
```
In output, a set of features having a + and a - infront of them will be shown. For example, Vim if complied with Python 2 support will dispaly (+python) instead of Python3 support (-python3)

I am not gonna talk about the .vimrc files. It is a different world. It is the configuration file for Vim where we can configure things like syntax highlighting, consistent indentation etc.
>`I've been using Vim for about 2 years now, mostly because I can't figure out how to exit it.`

***
 
## Interacting with Vim
* Line Starting with **double quote** " are comments and are ignored by Vim
* Pressing colon : character enters a command line mode     
* **Swap Files** keep track of the changes made and can be used to recover if Vim is not closed properly, hit r to recover swap file or d to delete it
* `:e` [ dit ], :e file_name
* `:w`[ rite ]
* `:q` [ uit ]
* `:q!` [ force quit without saving ] 
* `:wq` [ Combines two commands write & Quit ]

## Navigation Key
* **Moving around** h: Left arrow, j: Down arrow, k: Up arrow, l: Right Arrow
* **word** is a sequence of letters, digits, and underscores separeted by white space while **WORD** is a sequence of any non-blank characters separated by white space
* **w** Move forward by word
* **e** Moved forward until the end of the word
* **b** Move backwards to the beginning of the word
* **W** Move forward by WORD
* **E** Moved forward until the end of the WORD
* **B** Move backwards to the beginning of the WORD

![Navigation](/assets/img/post/navigation.webp)

* **{** Move back by one paragraph
* **}** Move forward by one paragraph

## Making Simple Edit in Insert Mode
* **c** Stands for Change, Compound Command ( a command that needs to be followed by another command), Example: cw cb c3e c4l cW
* **d** Stands for delete, Example: dw db dW
* **cc** Clears the whole line and enters insert mode. Preserves current indentation level, which is
useful when coding
* **dd** Deletes an entire line

## Persistent undo and repeat
* **u** to undo a last operation
* **Ctrl+r** to redo it

Vim also allows you to persist undo history between sessions, which is great if you want to undo (or remember) something you've done a few days ago! You can enable persistent undo by adding the following line to your .vimrc: `set undofile`
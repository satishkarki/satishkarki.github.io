---
layout: post
title: "Linux: Power To The Penguin"
date: 2023-07-28 20:00:00
categories: Linux
tags: process file_system  # TAG names should be lowercase
image:
    path: /assets/img/headers/linux.webp
---
Linux users are a bit like wizards of the digital realm. They conjure up commands from their spellbooks (the terminal), casting incantations like 'sudo' to gain magical powers over their systems. And just like wizards, they might spend hours delving into their scrolls (documentation) to decipher ancient runes (code) and unlock the secrets of the command line.

## History
First released in 1991

## File System
```shell
echo #Output any text that we provide
whoami #Find out what  user  we're currently logged in as
ls #Listing
cd #changing the directory
cat #Concatenation- outputting the contents of a file
pwd #Print working directory
```
> Tips: Pro tip: You can use cat to output the contents of a file within directories without having to navigate to it by using cat and the name of the directory. i.e. cat /home/ubuntu/Documents/todo.txt

### Searching for files
```shell
find -name password.txt
find -name *.txt
wc -l example.txt #counts the newlines in example.txt file
grep "10.1.216.10" access.log #search for "10.1.216.10"
```
### shell operator
| Symbol / Operator | Description                                                                                                                                      |
|-------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| &                 | This operator allows you to run commands in the background of your terminal.                                                                     |
| &&                | This operator allows you to combine multiple commands together in one line of your terminal.                                                     |
| >                 | This operator is a redirector - meaning that we can take the output from a command (such as using cat to output a file) and direct it elsewhere. |
| >>                | This operator does the same function of the > operator but appends the output rather than replacing (meaning nothing is overwritten).            |
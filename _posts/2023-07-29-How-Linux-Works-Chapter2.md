---
layout: post
title: "How Linux Works - Chapter 2 Notes"
date: 2023-07-29
categories: linux 
image:
    path: /assets/img/headers/linux.webp
---

This note is based  on the book -  `How Linux Works` 3rd edition - by Brain Ward. I find it one of the best book for anyone trying to learn linux. With that said, I wish sometime there was a little bit more explanation of the some of the topics in depth. Thats when `Unix and Linux System Administration Handbook` comes in handy. It is the holy bible of linux. With 1180 pages, I only occasionally visit some topics but most of the time it is a shelf decorator.

The content below is the notes/summary from Chapter 1 and 2 of the book and mostly about basic commands.

## The Bourne Shell : /bin/sh

Linux uses an enhanced version of the Bourne shell called bash or the "Bourne-again" shell. `/bin/shell` is a link to bash in Linux. 
```bash
# To change Shell
chsh 
```
```bash
$ cat /etc/shells
# /etc/shells: valid login shells
/bin/sh
/usr/bin/sh
/bin/bash
/usr/bin/bash
/bin/rbash
/usr/bin/rbash
/usr/bin/dash
/usr/bin/tmux
```
```shell
# MAN page for chsl
$ chsh -h
Usage: chsh [options] [LOGIN]

Options:
  -h, --help                    display this help message and exit
  -R, --root CHROOT_DIR         directory to chroot into
  -s, --shell SHELL             new login shell for the user account
```
```bash
# Example
$ chsh -s /usr/bin/tmux
```
---

## Basic Commands

```bash
cat # Output the contents of one or more file
$ cat /etc/passwd
```
```bash
$ ls # List the content of a directory
$ ls --help
$ ls -l #detailed long list
$ ls -R # list subdirectories recursively
$ ls -a # Don't ignore entries starting with .
```
```bash
$ cp file1 file2
$ cp file dir
$ cp file1 file2 file3 dir
```
```bash
$ mv file1 file2
```
```bash
$ touch file
```
```bash
$ rm file
```
```bash
$echo Hello Again.
Hello Again.
```
---

### Navigating Directories
```bash
/ -> root
../ -> parent directory
./ -> Current directory
```
```bash
$ cd dir # Change Directory
$ mkdir dir # Make Directory
$ rmdir dir # Remove Directory
```
---
### Shell Globbing
```bash
> at* expands to all files that start with at
> *at expands to all the files that end with at
> *at* expands to all files that contains at
> b?at expands to match exactly one arbitrary character # b?at matches boat and brat
```
---
## Intermediate Command
### grep
```bash
$ grep root /etc/passwd
# Output
root:x:0:0:root:/root:/bin/bash
nm-openvpn:x:121:122:NetworkManager OpenVPN,,,:/var/lib/openvpn/chroot:/usr/sbin/nologin
```

It lets display the content of a very large file one screenful at a time. To go to next page press space, to go back, press b, to quit q
```bash
$ grep ie /usr/share/dict/words | less
```
---

```bash
# print working directory
$ pwd
```
```bash
# difference between two files
$ diff file1 file2
```
```bash
# Check file or directory
$ file CCNA
CCNA: directory

$ file ./CCNA/CISCO\ commands 
./CCNA/CISCO commands: very short file (no magic)
```
---
### Find
```bash
# Find and Locate
$ find dir -name file -print
# Example
find /var/www          -type f          -name "*.php"           -path "*/wp-content/*"
   │                        │                 │                        │
   │                        │                 │                        │
Where to start           Only regular      Filename must           Full path must
the search               files             end with .php           contain /wp-content/
```
Scenario
```bash
/var/www
├── site1
│   ├── wp-content          ← matches
│   │   └── themes
│   │       └── style.php
│   └── index.php           ← does NOT match (no wp-content in path)
├── site2
│   ├── cache               ← does NOT match
│   └── somefile.php
└── old-projects
    └── wordpress-2023
        └── wp-content      ← matches
            └── plugins
                └── plugin.php
```
```bash
# Find 
$ find /var/www -type f -name "*.php" -path "*/wp-content/*"

# Output
/var/www/site1/wp-content/themes/style.php
/var/www/old-projects/wordpress-2023/wp-content/plugins/plugin.php
```
```bash
# Head and Tail
$ head /etc/passwd # First 10 lines
$ tail /etc/passwd #  Last 10 lines

$ head -5 /etc/passwd # First 5 lines only
```
---
### Shell Variable and Environment Variable
```bash
# 1. Normal shell variable (local only)
count=42

echo $count          # → 42
bash                 # open new shell
echo $count          # → (empty)
exit                 # back to original shell
echo $count          # → 42 (still exists here)

# 2. Environment variable (inherited)
export NAME="John"

echo $NAME           # → John
bash                 # new shell
echo $NAME           # → John (inherited!)
exit

# 3. Most common real-life pattern
export PATH="$HOME/bin:$PATH"           # add your own bin folder permanently
export EDITOR=nano                      # set default editor
export LANG=en_US.UTF-8                 # set language
```
```bash
# Command Path
$ echo $PATH # list of folders your shell searches
/usr/local/sbin:/usr/local/bin:...

# type: tells you what the shell will actually run when you type a command
$ type ls
ls is aliased to `ls --color=auto`

$ which ls
/usr/bin/ls

$ which -a ls
/usr/bin/ls
/bin/ls
```
```bash
# Modifying Path -Temporary
# Disappears after session is closed

# Add a directory at the BEGINNING (highest priority)
export PATH="/new/path:$PATH"

# Add at the END (lowest priority)
export PATH="$PATH:/new/path"

# Example - add your personal bin folder
export PATH="$HOME/.local/bin:$PATH"
```
```shell
# Permanent Modification
# 1. First check which shell you are using
$ echo $SHELL
/bin/bash

# 2. Edit Shell's start up file
# For bash users (most common on Ubuntu, Debian, Fedora, etc.)
$ nano ~/.bashrc
#     or
$ vim ~/.

# 3. Add path
# Option A - Add at the beginning (recommended - your programs first)
$ export PATH="$HOME/.local/bin:$PATH"

# Option B - Add at the end (system programs first)
$ export PATH="$PATH:$HOME/my-custom-scripts"

# Option C - Multiple additions
$ export PATH="$HOME/bin:$HOME/.local/bin:/opt/mytool/bin:$PATH"

# 4. Apply the changes
$ source ~/.bashrc

# 5. Verify
$ echo $PATH
```
---
### Command Line Editing
```bash
# Holy trinity — 80% of your editing power
Ctrl + A          → beginning
Ctrl + E          → end
Ctrl + W          → delete last word
Ctrl + U          → delete everything before cursor
Ctrl + K          → delete everything after cursor
Ctrl + R          → search history (type and press repeatedly)
Ctrl + L          → clear screen (keep command)

# Bonus power combo
Ctrl + W Ctrl + Y → cut last word → paste it somewhere else
```
---
### man page
Online Manual Section

| Section | What it contains                          | Typical examples                              |
|---------|-------------------------------------------|-----------------------------------------------|
| 1       | Executable programs / commands            | ls, grep, cat, passwd (the command)           |
| 2       | System calls (kernel functions)           | fork, open, read, write                       |
| 3       | Library functions                         | printf, strlen, malloc                        |
| 4       | Special files (devices)                   | /dev/sda, /dev/null                           |
| 5       | File formats and conventions              | passwd, shadow, hosts, crontab                |
| 6       | Games                                     | tetris, nethack                               |
| 7       | Miscellaneous (protocols, standards, etc.)| regex, utf-8, systemd.unit                    |
| 8       | System administration commands            | useradd, systemctl, fdisk                     |

```bash
man passwd # shows the command passwd (how to change passwords) → section 1
man 5 passwd # shows the file format of /etc/passwd → section 5
```
---
### Shell Input & Output
```shell
$ command > file # erase content of file and add the command
$ command >> file # append

$ head < /proc/cpuinfo 

$ head /proc/cpuinfo | tr a-z A-Z # tr command translate all cpu info from lowercase to upper case
```
---
### Standard Error
```bash
# First command
ls /ffff > f 2>e

# After running:
ls -l
-rw-r--r-- 1 user user  0 ... f     ← empty file
-rw-r--r-- 1 user user 39 ... e     ← contains the error
cat e
ls: cannot access '/ffff': No such file or directory


# Second command (the classic one)
ls /ffff > f 2>&1

# After running:
ls -l
-rw-r--r-- 1 user user 39 ... f     ← only one file, contains the error
cat f
ls: cannot access '/ffff': No such file or directory
```
---

## `/etc/sudoers`
Instead of using nano, vim or any other file editor , always use `visudo` command to edit the file.

```bash
$ sudo visudo

# Output
...
...
# Host alias specification

# User alias specification

# Cmnd alias specification

# User privilege specification
root    ALL=(ALL:ALL) ALL

# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# See sudoers(5) for more information on "@include" directives:

@includedir /etc/sudoers.d
```
```bash
# to add user
sudo usermod -aG sudo username 
# or
sudo adduser username sudo
```
Modern sudo includes all files from `/etc/sudoers.d/`. This is cleaner than editing the main `/etc/sudoers` file, especially on managed systems.

**Example**

1. Name the file after the user or purpose `/etc/sudoers.d/karking`
   
    ```bash
    $ sudo visudo -f /etc/sudoers.d/username 
    # -f tells visudo to edit this specific file instead of the default /etc/sudoers
    ```
2. Modify the file
   
   ```bash
   karking ALL=(ALL:ALL) ALL # Full sudo (like members of the sudo group)

   karking ALL=(ALL) NOPASSWD: ALL # Full sudo, no password prompt (use sparingly!)

   karking ALL=(ALL) NOPASSWD: /usr/bin/apt update, /usr/bin/apt upgrade # Only specific commands, no password

   karking ALL=(postgres) NOPASSWD: /usr/bin/psql # Run as another user (e.g., as postgres)
   ```
3. Quick verification command
   ```bash
   sudo visudo -c -f /etc/sudoers.d/username # Just syntax-check this file
   sudo visudo -c # Check entire config
   ls -l /etc/sudoers.d/ # Should show -r--r----- root root
   sudo whoami
   ```
---










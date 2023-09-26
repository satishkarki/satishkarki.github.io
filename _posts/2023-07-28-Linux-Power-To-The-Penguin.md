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
![File System](/assets/img/post/Linux-Power-To-The-Penguin/FileSystem.png)
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
```bash
& #This operator allows you to run commands in the background of your terminal
&& #This operator allows you to combine multiple commands together in one line of your terminal
> #This operator is a redirector - meaning that we can take the output from a command (such as using cat to output a file) and direct it elsewhere
>> #This operator does the same function of the > operator but appends the output rather than replacing (meaning nothing is overwritten)

```

>Operator "&&" : This shell operator is a bit misleading in the sense of how familiar is to its partner "&". Unlike the "&" operator, we can use "&&" to make a list of commands to run for example command1 && command2. However, it's worth noting that command2 will only run if command1 was successful.


```bash
ssh username@ipaddress
# SSH allows us to remotely execute commands on another device remotely.
# Any data sent between the devices is encrypted when it is sent over a network such as the Internet
```
### Flags and Switches
A majority of commands allow for arguments to be provided. These arguments are identified by a hyphen and a certain keyword known as flags or switches.

```bash
ls -a # Short for -all, files and folders with "." are hidden files.
```
### Manual Page
```bash
man ls # Manual page for ls
```
### File System Interaction
```bash
touch filename #Create file
mkdir Folder1 #Create folder
cp oldfile newfile #Copy a file or folder
mv oldfolder newfolder #Move a file or folder
rm -r folder #Remove a file or folder
file name #Determine the type of a file
```

>   Tips: Similarly to using cat, we can provide full file paths, i.e. directory1/directory2/note for all of these commands

> rm is extraordinary out of the commands that we've covered so far. You can simply remove files by using rm. However, you need to provide the -R switch alongside the name of the directory you wish to remove.

## Permission 101
![Permission 101](/assets/img/post/Linux-Power-To-The-Penguin/permission101.png)

In this example, the columns represent the following information:

- **File Permissions**: These indicate the read, write, and execute permissions for the owner, group, and others.
- **Number of Links**: This indicates the number of hard links to the file or directory.
- **Owner**: The user who owns the file or directory.
- **Group**: The group associated with the file or directory.
- **File Size**: The size of the file or directory in a human-readable format.
- **Date and Time**: The date and time of the last modification.
- **Name**: The name of the file or directory.

![Linux File Permission](/assets/img/post/Linux-Power-To-The-Penguin/Linuxfilepermission.png)

## General Useful Utilities

```bash
wget #downloading file
SCP (SSH) #transferring files from your host
python3 -m http.server #Serving Files from your host-web
```
### Secure Copy 
```bash
scp myfile.txt user@remote-server:/path/to/destination/ #Copy a local file to a remote server
scp user@remote-server:/path/to/remote-file.txt  /local/path/ #Copy a remote file to your local machine
scp -r mydir/ user@remote-server:/path/to/destination/ #Copy a directory from your local machine to a remote server
scp -r user@remote-server:/path/to/remote-dir/ /local/path/ #Copy a directory from a remote server to your local machine
```
## Process 101

```bash
ps aux
```
- **ps**: This is the command used to list processes.
- **aux**: These are the options or flags that modify the behavior of the ps command:
    - **a**: Show processes for all users. By default, ps only shows processes associated with the current user
    - **u**: Display detailed user-oriented format information. This includes the user who owns the process (UID), the CPU and memory usage, the start time, and the command that initiated the process
    - **x**: Include processes that are not attached to a terminal (i.e., background processes or daemons)

![Linux Process](/assets/img/post/Linux-Power-To-The-Penguin/process.png)

In this output:
 
- **USER**: The username of the process owner.
- **PID**: The process ID, a unique identifier for each process.
- **%CPU**: The percentage of CPU time the process is using.
- **%MEM**: The percentage of RAM (memory) the process is using.
- **VSZ**: The virtual memory size in kilobytes (KB).
- **RSS**: The resident set size, which is the portion of the process's memory that is held in RAM.
- **TTY**: The terminal associated with the process (or "?" if none).
- **STAT**: The process state (e.g., R for running, S for sleeping, Z for zombie).
- **START**: The start time of the process.
- **TIME**: The total accumulated CPU time used by the process.
- **COMMAND**: The command that initiated the process.

### top
![top](/assets/img/post/Linux-Power-To-The-Penguin/top.png)

When you run the top command in your terminal, you'll see a live, continuously updated screen with various information and statistics. Here's a breakdown of the key components of the top interface:
 
- Header Information: The top line of the screen displays system information such as the current time, system uptime, number of users, load averages, and more.
- Summary Information: The second line provides a summary of the system's CPU usage, including user processes, system processes, idle percentage, and more.
- Process List: Below the summary information, there's a list of processes. Each line represents a process and displays information such as process ID (PID), CPU usage percentage, memory usage, command name, and more.
- Interactive Commands: The top interface is interactive. You can use various keys to perform actions such as sorting the process list, changing the update interval, and sending signals to processes.
- Help and Commands: Pressing the h key within the top interface will display a help screen that explains the available interactive commands.
 
Here are some commonly used interactive commands within the top interface:
 
- **q**: Quit the top program.
- **k**: Kill a process. You'll be prompted to enter the PID of the process you want to terminate.
- **s**: Change the update interval.
- **P**: Sort the process list by CPU usage.
- **M**: Sort the process list by memory usage.
- **u**: Display processes for a specific user.
- **f**: Select and add or remove fields to customize the process display.

### Getting Processes/Services to Start on Boot
**systemctl** command allows us to interact wiht the systemd
```bash
sudo systemctl start apache2 #Starting a Service
sudo systemctl stop apache2 #Stopping a Service
sudo systemctl restart apache2 #Restarting a Service
sudo systemctl status apache2 #Checking the Status of a Service
sudo systemctl enable apache2 #Enabling a Service at Boot
sudo systemctl disable apache2 #Disabling a Service at Boot
systemctl list-units --type=service #Listing All Services
sudo systemctl reload apache2 #Reloading a Service Configuration
```
### backgrounding and foreground
> long-running-command & Ctrl+Z key combination to suspend the process and then using the bg command to resume it in the background.

> fg foregrounding

### Maintianing your system- Automation
Crontab is one of the processes that is started during boot, which is responsible for facilitating and managing cron jobs.

![cron job](/assets/img/post/Linux-Power-To-The-Penguin/cronjob.png)
```bash
crontab -e #To edit cron job
crontab -I # To view cron job

#Example
30 2 * * * /path/to/backup.sh
#The first field (30) is the minute.
#The second field (2) is the hour.
#The third field (*) represents any day of the month.
#The fourth field (*) represents any month.
#The fifth field (*) represents any day of the week.
#So, the task runs at 2:30 AM every day.
```

## Managing your repository
Advanced package Tool (apt)
```bash
sudo apt install package-name #Installing packages
sudo apt upgrade package-name #Upgrading Packages
sudo apt update #Updating package information
sudo apt remove package-name #Removing packages
apt search keyword #Searching for packages
apt list --installed #Listing installed packages
sudo apt autoremove #Cleaning up unneeded packages
sudo show package-name #Showing package information
```

Here is a beautiful website to start your [Linux Journey](https://linuxjourney.com/)

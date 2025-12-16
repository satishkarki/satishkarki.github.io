---
layout: post
title: "How To Manage Debian Packages"
date: 2021-08-16
categories: Linux
---

------

**You can download anything from the web browser. So, why go through the trouble of downloading it in the terminal ? Well, next time you will have the superpower to download anything, the cool way.**

The command I am using is **wget** and the URL passed to it is the repository location of **.deb** file.

```
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
```

**wget command syntax**

```
wget [options] [URL]
```

| OPTIONS                    | DESCRIPTION                                                  |
| :------------------------- | :----------------------------------------------------------- |
| wget -O *filename* URL     | Saving the downloaded file under a different name            |
| wget -P *file path* URL    | Downloading a File to a Specific Directory                   |
| wget --limit-rate=*1m* URL | Limiting the download speed , 1m means 1 megabyte, 1k means 1 kilobyte, 1g means 1 gigabyte |
| wget -c URL                | Resuming a download                                          |
| wget -b URL                | Downloading in Background                                    |
| tail -f wget-log           | By default, the output is redirected to wget-log file in the current directory. To watch the status of the download, use the tail command |

These were some of the basic options with wget. We can change the Wget User-Agent, download multiple files, download via FTP, Create a mirror of a Website, skip certificate check, download to the standard output. Here is the [GNU Wget's dirty manual](https://www.gnu.org/software/wget/manual/wget.html).

---

Now we have downloaded the file, we need to install the Debian package. **dpkg** is the package manger for Debian. 

```
dpkg [options...] action
```

| Command                                | Function                                                     |
| -------------------------------------- | ------------------------------------------------------------ |
| dpkg -i *package_file.deb*             | Install package                                              |
| sudo apt -f install                    | to fix the unmet dependency issues once a debian package is installed |
| dpkg -I */path/to/file.deb*            | List all dependencies of a Deb file                          |
| dpkg-deb -c */path/to/file.deb*        | List all the files that will be installed from a deb package |
| dpkg-deb --extract */path/to/file.deb* | Extract all files from a deb package                         |

Learn more about it [here](https://linuxhint.com/manual_install_deb_package_cli_ubuntu/) .

---

We can use apt to install the Debian package as well

```
apt 
```

Most of the apt command must be run as a user with sudo privileges. apt-get and apt-cache are backward compatible between the different versions and have more options and features.

| Command                                            | Function                                                     |
| -------------------------------------------------- | ------------------------------------------------------------ |
| sudo apt list                                      | Listing packages                                             |
| sudo apt list --installed                          | List installed packages only                                 |
| sudo apt search package_name                       | Searching packages                                           |
| sudo apt show package_name                         | Package Information                                          |
| sudo apt list --upgradeable                        | List all upgrade able packages                               |
| sudo apt update                                    | Updating package index                                       |
| sudo apt upgrade                                   | upgrading the packages to their latest versions              |
| sudo apt upgrade *package_name*                    | upgrade a single package                                     |
| sudo apt full-upgrade                              | It will remove the installed packages if that is needed to upgrade the whole system |
| sudo apt install *Package_name 1* *Package_name 2* | Installing packages                                          |
| sudo apt install */full/path/file.deb*             | To install local deb file                                    |

---

Let's delete the packages now

| Command                      | Function                                                     |
| ---------------------------- | ------------------------------------------------------------ |
| sudo apt remove package_name | Removing Package                                             |
| sudo apt purge package_name  | It will remove package file including all configuration files |
| sudo apt autoremove          | Remove Unused Packages                                       |

Learn more about apt [here](https://linuxize.com/post/how-to-use-apt-command/[)


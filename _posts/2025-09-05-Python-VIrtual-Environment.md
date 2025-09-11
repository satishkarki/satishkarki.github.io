---
layout: post
title: "Python-Virtual Environment"
date: 2025-09-05 10:00:00
categories: Python
tags: python  # TAG names should be lowercase
image:
    path: /assets/img/headers/virtualenv.webp
---
# Python Setup and Virtual Environment

In MacBook, open Terminal and run `python3-- version` to check Python 3. It is unlikely that Python 2 is installed, but you can check with `python-- version`. If no Python is installed, the prompt to install Xcode tools will appear, confirming that Python 3 is not preinstalled for user access by default.

## Install a Custom Python Version in MacBook:

Apple’s Python (from Xcode tools) is fine for basic use but may be outdated or restricted for development. Developers typically install a separate Python version using:

* Homebrew: `brew install python@3.12` installs Python 3.12 (or the latest available) in `/opt/homebrew/bin` (on Apple Silicon) or `/usr/local/bin` (on Intel).

* pyenv: A tool to manage multiple Python versions, e.g., `pyenv install 3.12.4.`

* Official Installer: Download Python from `python.org` for a specific version.
  These methods give you control over updates and avoid conflicts with system tools.

* Check Python path in Windows
  
  ```shell
  C:\Users\satish.karki>where python
  C:\Users\satish.karki\AppData\Local\Programs\Python\Python313\python.exe
  C:\Users\satish.karki\AppData\Local\Microsoft\WindowsApps\python.exe
  ```

## Virtual Environments:

### In Windows

Use `venv` (built-in) or `virtualenv` (third-party) to isolate project dependencies, e.g., `python3 -m venv myenv`.

```shell
python -m ensurepip # Check if pip is installed
python -m pip install --upgrade pip #upgrade pip
```

```shell
# In your project directory, run
python -m venv venv # It creates a venv folder in your project directory

#Using bat file
venv\Scripts\activate

#Using Powershell Script
.venv\Scripts\activate 

#Output
(venv) C:\Users\satish.karki\OneDrive\Notes\python\Projects\Project1>where python

C:\Users\satish.karki\OneDrive\Notes\python\Projects\Project1\venv\Scripts\python.exe

#Export installed package to a file
pip freeze > requirements.txt
```

### git work

```shell
#In Windows
git add .
git commit -m "Inital commit"
git remote add origin https://github.com/satishkarki/pythonplayground.git
git branch -M main
git push -u origin main

echo. > .gitignore # Creates .gitignore
notepad .gitignore

git status

git add .gitignore
git commit -m "Ignore venv in .gitignore
git push origin main

#In MacBook
git pull origin main
git status
rm -rf venv #remove venv if it appears (it appeared for me becasue of OneDrive)
```

### In MacBook

```bash
which python 
/usr/bin/python3 #System Python
```

Modifying it (e.g., installing global packages with `pip`) can break system functionality. Homebrew is a popular package manager for macOS that installs Python in a separate location (e.g., `/opt/homebrew/bin` on Apple Silicon or `/usr/local/bin` on Intel), leaving the system Python intact.

```bash
brew search python # Check the latest Python version available
brew install python@3.12 #Install the 3.12 version
which python3.12

Output: /usr/local/bin/python3.12
```

Optional Step to update the path to prioritize Homebrew's Python over system Python

```bash
echo 'export PATH="/opt/homebrew/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

For Intel Macs, use `usr/local/bin` instead of `/opt/homebrew/bin`

Now lets create a virtual  environment

```bash
python3.12 -m venv venv
source venv/bin/activate #Activate venv
# output ((venv)) macbook@MacMan Project1%


pip install -r requirements.txt
pip list #to check the packages
pip freeze > requirements.txt #pin your dependencies



# For Windows
py -3.12 -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
```

## What is a Python Virtual Environment
When you create a new virtual environment using the venv module, Python creates a self-contained folder structure and copies or symlinks the Python executable files into that folder structure.

> ***Welcome to the dark abyss, brave one.***
```shell

$ brew install tree #If tree is not installed
$ tree venv/ #The tree command displays the content of your venv directory in a very long tree structure.

venv/
│
├── bin/
│   ├── Activate.ps1
│   ├── activate
│   ├── activate.csh
│   ├── activate.fish
│   ├── pip
│   ├── pip3
│   ├── pip3.12
│   ├── python
│   ├── python3
│   └── python3.12
│
├── include/
│
├── lib/
│   │
│   └── python3.12/
│       │
│       └── site-packages/
│           │
│           ├── pip/
│           │
│           └── pip-24.2.dist-info/
│
└── pyvenv.cfg
```
- **bin/** contains the executable files of your virtual environment. Most notable are the Python interpreter (python) and the pip executable (pip), as well as their respective symlinks (python3, python3.12, pip3, pip3.12). The folder also contains activation scripts for your virtual environment. 
- **include/** is an initially empty folder that Python uses to include C header files for packages you might install that depend on C extensions.
- **lib/** contains the site-packages/ directory nested in a folder that designates the Python version (python3.12/). site-packages/ is one of the main reasons for creating your virtual environment. This folder is where you’ll install external packages that you want to use within your virtual environment. 
- **pyenv.cfg** is a crucial file for your virtual environment. It contains only a couple of key-value pairs that Python uses to set variables in the sys module that determine which Python interpreter and site-packages directory the current Python session will use.
  ```shell
  #Contain of pyenv.cfg
  home = /usr/local/opt/python@3.12/bin
  include-system-site-packages = false
  version = 3.12.11
  executable = /usr/local/Cellar/python@3.12/3.12.11/Frameworks/Python.framework/Versions/3.12/bin/python3.12
  command = /usr/local/opt/python@3.12/bin/python3.12 -m venv /Users/macbook/Library/CloudStorage/OneDrive-Personal/Notes/python/Projects/pythonplayground/venv
  ```
```shell
$ python3 -m venv venv/ --system-site-packages
```
Python will set the value to include-system-site-packages in pyvenv.cfg to true. This setting means that you can use any external packages that you installed to your base Python as if you’d installed them into your virtual environment. **This connection works in only one direction.**

## How can you customize a virtual environment?

```shell
$ python3 -m venv your-fancy-name/ #Command to create Virtual Environment
$ source your-fancy-name/bin/activate
(your-fancy-name) $

# To show descriptive string without changing the name of your virtual environment's folder
$ python3 -m venv venv/ --prompt dev-env
$ source venv/bin/activate
(dev-env) $

# Explicitly overwrite an exisitng virtual environment using --clear
$ python3 -m venv venv/
$ venv/bin/pip install requests
$ venv/bin/pip list
Package            Version
------------------ ---------
certifi            2024.8.30
charset-normalizer 3.3.2
idna               3.8
pip                24.2
requests           2.32.3
urllib3            2.2.2

$ python3 -m venv venv/ --clear
$ venv/bin/pip list
Package    Version
---------- -------
pip        24.2

# Create multiple virtual environments at once
# venv/ is using relative path
# venv-copy/ is using absolute path
$ python3 -m venv venv/ /Users/name/virtualenvs/venv-copy/ 

# Update the core dependencies
$ python3 -m venv venv/ --upgrade-deps
$ source venv/bin/activate
(venv) $ python -m pip install --upgrade pip
Requirement already satisfied: pip in ./venv/lib/python3.12/site-packages (24.2)

# Avoid installing pip
$ python3 -m venv venv/ --without-pip
$ du -hs venv/ # du:Disk Usage, -h:human readable -s:summarize
28K venv

#Include the system site-packages
$ python3 -m venv venv/ --system-site-packages
$ source venv/bin/activate
(venv) $

#Contain of pyenv.cfg after including site-packages
home = /Library/Frameworks/Python.framework/Versions/3.12/bin
include-system-site-packages = true
...
```

## References
* [Python Virtual Environments: A Primer](https://realpython.com/python-virtual-environments-a-primer/#activate-it)
* [PEP 405- Python VIrtual Environments](https://peps.python.org/pep-0405/#specification)
* [venv- Creation of virtual environments](https://docs.python.org/3/library/venv.html)
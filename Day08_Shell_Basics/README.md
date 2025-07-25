# Day 8 – Shell & Bash Basics

## 🎯 Objectives
- Understand what a shell is and how it interacts with the Linux kernel
- Learn about different shell types with a focus on Bash
- Get familiar with the Bash environment, variables, and basic configurations

---

## 🐚 What is a Shell?

The **shell** is a command-line interface (CLI) that allows users to interact with the Linux system by executing commands. It's a bridge between the **user** and the **kernel**.

### Common Shells in Linux:

| Shell  | Description                                                           | Common OS Usage                  | Benefits / Features                                                                | Typical Use Case                                           |
| ------ | --------------------------------------------------------------------- | -------------------------------- | ---------------------------------------------------------------------------------- | ---------------------------------------------------------- |
| `sh`   | The original Bourne Shell, minimal and portable                       | Legacy Unix, POSIX systems       | Lightweight, high portability, POSIX-compliant                                     | Legacy scripts, portable scripts for all Unix-like systems |
| `bash` | Bourne Again SHell; default shell on most Linux distros               | Ubuntu, Debian, CentOS, RHEL     | Widely supported, scripting-friendly, command history, job control, tab completion | General-purpose scripting and administration               |
| `zsh`  | Z Shell; modern and extensible shell with scripting enhancements      | macOS (default), Arch, Kali      | Powerful autocompletion, plugin/theme support (Oh My Zsh), syntax highlighting     | Interactive terminal use and power-user environments       |
| `csh`  | C Shell with C-like syntax and scripting style                        | BSD systems (FreeBSD, OpenBSD)   | C-style scripting syntax, good for interactive use on BSD systems                  | BSD user scripting, historical use                         |
| `tcsh` | Enhanced version of `csh` with additional features                    | FreeBSD, OpenBSD, some Linux     | Command-line editing, history, tab-completion                                      | Replacement for `csh` with modern usability improvements   |
| `fish` | Friendly Interactive Shell with a focus on usability                  | Cross-platform, Linux/macOS      | Smart suggestions, autosuggestions, syntax highlighting, intuitive syntax          | Beginners, user-friendly interactive shell                 |
| `dash` | Debian Almquist Shell, a lightweight POSIX-compliant shell            | Debian, Ubuntu (for `/bin/sh`)   | Fast, low memory footprint, good for system boot scripts                           | `/bin/sh` for startup scripts and speed-critical tasks     |
| `ksh`  | KornShell, powerful scripting shell with features from `sh` and `csh` | AIX, Solaris, older UNIX systems | Arrays, arithmetic operations, job control                                         | Enterprise UNIX environments, legacy applications          |


> 💡 Most Linux distros use **bash** as the default shell.

---

## 🧠 Understanding Bash

**Bash** (Bourne Again Shell) is a powerful shell that supports scripting, variables, loops, conditions, and more. It is both a shell and a scripting language.

Check your shell:
```bash
echo $SHELL
```

Start an interactive Bash session (if not already in Bash):
```
bash
```
## 🗂️ Bash Files and Configuration
| File                              | Purpose                                                    |
| --------------------------------- | ---------------------------------------------------------- |
| `~/.bashrc`                       | User-specific Bash settings loaded in interactive sessions |
| `~/.bash_profile` or `~/.profile` | Executed on login                                          |
| `/etc/bash.bashrc`                | Global Bash configuration                                  |
| `/etc/profile`                    | Global environment variables and settings                  |


## 🧪 Basic Bash Commands

```
clear             # Clear the terminal
pwd               # Print current directory
ls -l             # List directory contents
cd /etc           # Change directory to /etc
history           # View command history
```


## 🧾 Bash Variables

### User-defined variables
```
name="Saud"
echo $name
```
### Environment variables

```
echo $HOME
echo $PATH
```
### Export a variable
```
export EDITOR=vim
```
> ⚠️ No spaces around = when assigning variables.



## 📁 Working with Paths
```
echo $PATH        # List of directories the shell searches for executables
which ls          # Shows path of the `ls` command
```
Add a new directory to PATH:

```
export PATH=$PATH:/opt/mybin
```
## 🖊️ Aliases
Simplify long commands:

```
alias ll='ls -la'
alias gs='git status'
```
To make it permanent, add it to your ~/.bashrc:

```
nano ~/.bashrc
```

### 📜 Quoting in Bash

| Syntax                    | Meaning                  |
| ------------------------- | ------------------------ |
| `"double quotes"`         | Allow variable expansion |
| `'single quotes'`         | No expansion, raw text   |
| \`backticks\` or `$(...)` | Command substitution     |


Example:
```
echo "User is $USER"
echo 'User is $USER'
echo "Today is $(date)"
```


## 🔄 Command Substitution
```
current_date=$(date)
echo "Current date: $current_date"
```


## 🧪 Practice Tasks
1. Create 2 variables and print them.

2. Use alias to shorten a common command.

3. Add a new alias to .bashrc and reload it with:

```
source ~/.bashrc
```
Use command substitution to show:
```
echo "The uptime is: $(uptime)"
```

### 📚 Resources

- [GNU Bash Manual](https://www.gnu.org/software/bash/manual/)

- [Bash Scripting Guide ](https://linuxconfig.org/bash-scripting-tutorial)

- [Explainshell – Visual command breakdown](https://explainshell.com/)







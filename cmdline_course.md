---
layout: default
---

## Introduction

The [Command-Line Tools for Linguists](https://studies.helsinki.fi/kurssit/toteutus/hy-opt-cur-2425-261401a1-c550-4436-91b9-7edf4a1a3b57/KIK-LG221)
course is an introduction to working with the terminal, from the perspective of performing tasks relevant for language technology.

It teaches the basics of using an Unix-like environment, but also teaches
* regular expressions
* running and installing programs from terminal
* shell scripting
* version control tools ([git](https://git-scm.com/) specifically)
* working with remote servers
* hosting a static website (like this) with [GitHub Pages](https://pages.github.com/)

Overall, I think it's a quite well made course that might be useful even for individuals with prior experience with Unix-like systems, like myself.

## Week 1 - Introduction to the command-line

This week was an introduction to using the command-line, focusing on how to perform the most basic computing tasks such as navigating between
directories and working with files, but in the terminal.

### Commands

| Command | Description |
| --- | --- |
| `ls` | List contents of an directory |
| `cd` | Change directory |
| `less` | Display contents of a file |
| `cat` | Print the contents of files to the standard output |
| `wget` | Download a file over the internet |
| `touch` | Create a file |
| `mkdir` | Create directories |
| `cp` | Copy files or directories |
| `mv` | Rename or move files (or directories) |
| `rm` | Remove files or directories |

### Editing files in a terminal text editor

* [nano](https://www.nano-editor.org/) was the terminal text editor advised to be used in the exercises
  ```bash
  nano file.txt
  ```

* [vim](https://www.vim.org/) is another terminal text editor one could use
  ```bash
  vim file.txt
  ```

  But beware...

  <img src="https://cdn.stackoverflow.co/images/jo7n4k8s/production/7a0bf96c6e3155ca56c74723cb0c0767517a4429-324x318.jpg?auto=format">

### What I learned
I was already quite familiar with the topics in week 1, but I used the opportunity to try out [emacs](https://www.gnu.org/software/emacs/) on week 1.

## Week 2 - Unix file system, processes and remote servers

### Unix file system
Week 2 delved deeper to the concept of Unix filesystem
* Symbolic links
  * Symbolic links are files that actually points to another file in some other location on the computer
* File permissions
  * Can be set differently on user and group level

### Processes
Week 2 also teached how processes works on Unix systems, and how to find and stop them.

| Command | Description |
| --- | --- |
| `ps` | Can be used to find a process |
| `kill` | Can be used to kill a given process |
| `killall` | Can be used to kill all processes that matches a given name |

### Remote servers
There was also a section about working with remote servers.

| Command | Description |
| --- | --- |
| `ssh` | Connect to a remote server |
| `scp` | Copy a file from the server to the local machine, or vice versa |

### What I learned
The most useful thing that I learned was from the materials about processes,
that processes can be started and put directly to a background process, e.g.
```bash
htop &
```

And then, that process can be brought back to the foreground with command
```bash
fg
```

I also discovered that the process can be put back to background
by pressing `ctrl + z`.

## Week 3 - Basic Corpus Processing

This week started to get more technical and relevant in terms of language technology.

### Commands introduced

| Command | Description |
| --- | --- |
| `grep` | Can be used to search patterns in given input |
| `tr` | Can be used to edit the input, e.g. replacing characters with other characters |
| `wc` | Can be used to e.g. print word counts for input |

### Pipelines
Pipelines was first introduced here. Pipelines are basically ways to combine commands,
so that the output of a command is given as an input to another command. For example

```bash
echo -e "First line\nSecond line" | grep "First" | tr "F" "f"
```

In the above example, the first `echo` command prints two lines.
Those two lines are passed to `grep`, which searches for a line containing the text "First".
Then, that line is given to `tr` command, which replaces the uppercase F character with lowercase f.

### What I learned
Although I had heard of and used `grep` before, I hadn't really realized how useful it is until now.
Before I had only actually used `grep` to find a word occurence in a file (e.g. `cat file.txt | grep keyword`)
but here I learned how it can be used to find matches of regular expressions.

A bit off-topic to this course, but I also found out that grep could be used as (at least a part of)
a do-it-yourself terminal file-picker, for example to find all files containing `keyword` in a directory:
```bash
grep -lR "keyword" ./ | fzf --preview 'less {}'
```
Here the output of grep is passed to [fzf](https://github.com/junegunn/fzf), a command-line fuzzy finder,
that gives a nice interface for selecting an option from a list given by grep.

This could even be extended by passing that whole thing to a text editor like `nano`:
```bash
nano $(grep -lR "keyword" ./ | fzf --preview 'less {}')
```

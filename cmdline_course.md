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
It was teached how to e.g. analyze a text file by generating a word frequency list out of it,
and then performing tasks on the word frequency list to find out e.g. how many words
in the text file ends with a sequence.

### Commands introduced

| Command | Description |
| --- | --- |
| `grep` | Can be used to search patterns in given input |
| `tr` | Can be used to edit the input, e.g. replacing characters with other characters |
| `wc` | Can be used to e.g. print word counts for input |
| `uniq` | Filters out repeated lines in input |

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

This has become a part of my everyday workflow now.

## Week 4 - Advanced Corpus Processing

This week extends from previous week, performing more challenging tasks to analyze
text files.
Here was also introduced how to transform a text file into a sentence per line format
by using `sed`, `tr` and `grep`.
It was also teached how to transform a text file to n-grams using `sed`.

### Commands introduced

| Command | Description |
| --- | --- |
| `diff` | Compares files line by line and highlights differences |
| `sed` | Stream editor for filtering and replacing text |

### Text processing

These tools can be used together to do in-depth analysis of text. For example,
to find the most common last word in text:
```bash
echo -e "First line\nSecond word\nThird word" | sed -E 's/.* //' | uniq -c | sort -nr | head -1
```

Here are three lines, first of which ends with the word "line", and the last two lines ends with "word".
We use `sed` to transform each line to last word only, then `uniq -c` to find how many occurences
there is of each last word. `sort -nr` sorts (in reversed order) based on the count, and `head -1` to get
the most common word.

Obviously this is a small example, but could be very useful for doing analysis e.g.
on a book or other larger text.

### What I learned
I learned that `sed` is very useful. I had seen it being used before, but didn't really understand it.
I could see myself using `sed` in the future if I need to automate text editing.

## Week 5 - Scripting and Configuration Files

This week introduced shell scripting and configuring bash.

### Shell scripts
Shell scripts are executable files. To make a file executable, one can run `chmod +x file`.

Shell scripts can be used to automate a set of commands by executing the script.

For example, to make a script that adds an exclamation mark to each line of a file, one could write a script
```bash
filename=$1
while read -r line;
do
  echo ${line}!
done < "$filename";
```

In the above example, the `filename` is given by user as an argument when executing that script (e.g. `./path/to/script.sh somefile.txt`).

### Configuration files
The `bash` shell can be configured using a `.bashrc` file in the home directory. For example it could be used to define aliases

```bash
# .bashrc
alias ga="git add ."
```

### What I learned
I didn't know about the `case...esac` statement of shell scripts. This seems to be very useful.

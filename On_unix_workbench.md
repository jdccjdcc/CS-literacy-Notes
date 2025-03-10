# On the Unix Workbench
content | link
| -- | -- |
| mooc | [CS literacy MIT](https://missing.csail.mit.edu/2020/command-line/) |
| mooc | [Unix workbench Coursera]()

---
table of contents
- [On the Unix Workbench](#on-the-unix-workbench)
  - [the Shell](#the-shell)
  - [Shell tools](#shell-tools)
    - [Shell scripting](#shell-scripting)
    - [finding files](#finding-files)
    - [finding code](#finding-code)
  - [Command-line Environment](#command-line-environment)
    - [job control](#job-control)
    - [dotfiles](#dotfiles)
    - [remote machines](#remote-machines)
    - [Checking differences in files](#checking-differences-in-files)
    - [Makefiles](#makefiles)




---
## the Shell
- `\` escape caracter

- If the shell is asked to execute a command that doesn’t match one of its programming keywords, it consults an **environment variable** called ```$PATH``` that lists which directories the shell should search for programs when it is given a command
- A path that starts with `/` is called an absolute path. Any other path is a relative path
- In a path, `.` refers to the current directory, and `..` to its parent directory

- The `|` "pipe" operator lets you “chain” programs such that the output of one is the input of another
```bash
curl --head --silent google.com | grep --ignore-case content-length | cut --delimiter=' ' -f2
```
result: ```219```.  
```curl```, a tool to transfer data from or to a server,  
 fetches metadata (--head option) and the **pipe** takes that output and passes it to   
 ```grep```, a command-line tool to search through text, looks in the headers for "content-lenght" and outputs the line(s) where it is found. **Pipe** passes that line(s) to  
 ```cut```, a tool to extract sections from lines of text, which in this case defines whitespace as delimiter and looks for the second field

 ```man``` - navigate the documentation page of a command:  
  After typing e.g. ```man ls``` to open the page, type ```/``` in order to start a search (e.g. you want to look for ```-l``` flag). Press the ```n``` key in order to search for the next occurrence of the word, and if you want to go to the previous occurrence type ```Shift + n```. This method of searching also works with ```less```. When you’re finished looking at a man page type ```q``` to get back to the prompt

## Shell tools

### Shell scripting
Shebang (`#!`) is used as the first line of a shell script. It's an instruction to the system "Run this script using this shell". It's not a command to be used on the command line.
It must be in the first line of the script.

To write a portable shebang for a Zsh script, you need to account for the fact that Zsh may be installed in different locations on different systems.
- Best practices dictate using the `env` command in the shebang to dynamically locate the Zsh binary e.g. ```#!/usr/bin/env zsh``` is the most portable and reliable option, and the consistency of ```/usr/bin/env``` across platforms makes it the preferred approach.
- `env` command is used to find and run the interpreter (zsh in this case) by searching the system's PATH. This way, it doesn't rely on the interpreter being located at a fixed path like /bin/zsh or /usr/bin/zsh, which can vary across different systems.

`/usr/bin/env` is a *de facto standard* for the location of `env` across Unix-like systems. It’s one of the most reliable assumptions you can make for general portability.
Hence, the portability issue of the interpreter (zsh, or python for example) is solved and not just transfered to ```env```.

### finding files

+ `find` will recursively search for files matching some criteria
```bash
# Find all directories named src
find . -name src -type d
# Find all python files that have a folder named test in their path
find . -path '*/test/*.py' -type f
# Find all files modified in the last day
find . -mtime -1
# Find all zip files with size in range 500k to 10M
find . -size +500k -size -10M -name '*.tar.gz'
```
```bash
# Delete all files with .tmp extension
find . -name '*.tmp' -exec rm {} \;
# Find all PNG files and convert them to JPG
find . -name '*.png' -exec convert {} {}.jpg \;
## command line environment
```

### finding code
 - ```grep```(global regular expression print) - a generic tool for matching patterns from the input text
    - some flags:
    ```-C``` for getting Context around the matching line and ```-v``` for inverting the match, i.e. print all lines that do not match the pattern. ```-R``` will Recursively go into directories and look for files for the matching string.
 - ```rp``` ripgrep, a faster and more intuitive grep alternative
  ```bash
  # Find all python files where I used the requests library
  rg -t py 'import requests'
  # Find all files (including hidden files) without a shebang line
  rg -u --files-without-match "^#\!"
  # Find all matches of foo and print the following 5 lines
  rg foo -A 5
  # Print statistics of matches (# of matched lines and files )
  rg --stats PATTERN
  ```


## Command-line Environment
### job control
+ ```Ctrl-C``` to interrupt a job while it is executing
  + when typing Ctrl-C this prompts the shell to deliver a SIGINT signal to the process.
+ To kill a program we can use the SIGQUIT signal instead, by typing ```Ctrl-\```
+ typing ```Ctrl-Z``` will prompt the shell to send a SIGTSTP signal, short for Terminal Stop (i.e. the terminal’s version of SIGSTOP)
+ ```Ctrl+R``` to perform backwards search through your history.
+ A nice addition on top of Ctrl+R comes with using ```fzf``` bindings. fzf is a general-purpose fuzzy finder that can be used with many commands.
---
+ Commands will take input from both **arguments** and **STDIN**. When piping commands, we are connecting STDOUT to STDIN, but some commands like tar take inputs from arguments. To bridge this disconnect there’s the ```xargs``` command which will execute a command using STDIN as arguments.  
  For example ```ls | xargs rm``` will delete the files in the current directory.


### dotfiles
Many programs are configured using plain-text files known as dotfiles (because the file names begin with a ```.```, e.g. ```~/.zshrc```, so that they are hidden in the directory listing ls by default).

git - ~/.gitconfig
vim - ~/.vimrc and the ~/.vim folder
ssh - ~/.ssh/config

### remote machines
```ssh``` (Secure Shell)
- If you need to use remote servers in order to deploy backend software or you need a server with higher computational capabilities, you will end up using a Secure Shell (SSH)

### Checking differences in files

- Checksum or hash
- The ```md5``` and ```shasum``` commands use different algorithms to create codes (called hashes or checksums) that are unique to the contents of a file.
- These hashes can be used to ensure that a file is genuine.

### Makefiles
- ```make``` is a tool for creating relationships between files and programs, so that files that depend on other files can be automatically rebuilt.
- makefiles are text files that contain a list of rules.
- Rules are made up of targets (files to be built), commands (a list of bash commands that build the target), and dependencies (files that the target depends on to be built).

```txt  
[target]: [dependencies...]
  [commands...]
```
makefile e.g.:
```
all: draft_journal_entry.txt readme.txt

draft_journal_entry.txt:
  touch draft_journal_entry.txt
  
readme.txt: toc.txt
  echo "This journal contains the following number of entries:" > readme.txt
  wc -l toc.txt | egrep -o "[0-9]+" >> readme.txt
  ```

1.  ```cd``` into the directory
2. create the makefile and have the dependencies in the directory
3. run ```make [taget_file]``` or plainly ```make``` (if one creates a rule at the top of the makefile called **all** where we can list all files that are built by the makefile. By adding the **all** target we can simply run ```make``` without any arguments in order to build all targets in the makefile)

## Git and Github
Git is a VCS (version control system) that uses a **three-tree architecture**:
1. (git) repository;
2. working copy;
3. staging index.

In git files can exist in 3 stages:
- modified - file has changes but they were not yet commited to the local database
- staged - marked a modified version of a file to go into the next commit snapshot
- commited - data is safely stored in the local database

 Git object storage is a **DAG (Directed Acyclic Graph)** of objects, with a handful of different types of objects:
 - **blobs, trees, commits, tags** 
 
 They are all **stored compressed and identified by an SHA-1 hash**. -> Everything in Git is checksummed before it is stored and is then referred to by that checksum.

 + `References - pointers to commits`, whether they [references] are local branches, remote branches, or tags, **make commits**, which are nodes in a graph, **reachable**, so that it is possible to get back to them.

 On disk, a local branch reference consists entirely of a file, that contains the 40-byte identifier of the commit that the reference points to, in your project's .git/refs/heads directory.

 Because a git branch is so fast and unobtrusive, and branches are references that make commits reachable, creating a branch is a way to pin down part of the graph that might be important to come back later.

 Usually the HEAD file is a symbolic reference to the branch you’re currently on. By symbolic reference, we mean that unlike a normal reference, it contains a pointer to another reference.

 ```sh
cat .git/HEAD
#output: ref: refs/heads/master
```
if `git checkout test`, Git updates the file to look like this:
```sh
cat .git/HEAD
#output: ref: refs/heads/test
```



- setting up git and github:

```sh
git --version
```
```sh
git config --global user.name "myusername"
git config --global user.email myemail@email.com
```
---
```sh
git init
```

- Git tracks changes to plain text files (code files and text documents).
- A directory where changes to files are tracked by Git is called a Git repository.
- Change your working directory, then run `git init` to start a repository.
- You can **track changes** to a file using `git add [names of files]`.
- You can create a milestone about the state of your files using `git commit -m "message about changes since the last commit"`.
- To examine the state of files in your repository use `git status`.

#### Git commands:
  >Basics:
  - `git help <command>`: get help for a git command
  - ```git init```: creates a new git repo, with data stored in the .git directory
  - ```git status```: tells you what’s going on
  - ```git add <filename>```: adds files to staging area
  - ```git commit```: creates a new commit
  - `git log`: shows a flattened log of history
  - `git log --all --graph --decorate`: visualizes history as a DAG
  - `git diff <filename>`: show changes you made relative to the staging area
  - `git diff <revision> <filename>`: shows differences in a file between snapshots
  - `git checkout <revision>`: updates HEAD and current branch

  >Branching and merging:
  - ```git branch```: shows branches
  - ```git branch <name>```: creates a branch
  - ```git checkout -b <name>```: creates a branch and switches to it
  (same as git branch \<name>; git checkout <name>)
  - ```git merge <revision>```: merges into current branch
  - `git mergetool`: use a fancy tool to help resolve merge conflicts
  - `git rebase`: rebase set of patches onto a new base
  
  >Remotes:
  - `git remote`: list remotes
  - `git remote add \<name> \<url>`: add a remote
  - `git push <remote> <local branch>:<remote branch>`: send objects to remote, and update remote reference
  - `git branch --set-upstream-to=<remote>/<remote branch>`: set up correspondence between local and remote branch
  - `git fetch`: retrieve objects/references from a remote
  - `git pull`: same as `git fetch; git merge`
  - `git clone`: download repository from remote
  
  >Undo:
  - `git commit --amend`: edit a commit’s contents/message
  - `git reset HEAD <file>`: unstage a file
  - `git checkout -- <file>`: discard changes

## Nephology

### Automating tasks
 - a **cron program** is part of a famaly of programs called **daemons**
 - A daemon is a program that is always running in the background of our computer

 ```sh
 ps -A | "grep" cron
 ````
In order to assign programs to be executed with cron we need to edit a special text file called the cron table. Before we edit the cron table we need to select the default text editor.

```
select-editor

## Select an editor.  To change later, run 'select-editor'.
##   1. /bin/ed
##   2. /bin/nano        <---- easiest
##   3. /usr/bin/vim.basic
##   4. /usr/bin/vim.tiny
## 
## Choose 1-4 [2]:
```

```sh
crontab -e

# Edit this file to introduce tasks to be run by cron.
#
# m h  dom mon dow   command
```
simple examples of cron jobs:
```nano
# m h  dom mon dow   command
* * * * * date >> ~/date-file.txt           # Every minute writes the date in the specified file
00-04 * * * * bash /path/to/script.sh       # Runs every minute for the first five minutes of every hour
00 00 * * 0,6 bash /path/to/script.sh       # Runs at midnight every Saturday and Sunday
00 03 01-15 * * bash /path/to/script.sh     # Runs at 3am for the first fifteen days of every month 
00,30 * * * * bash /path/to/script.sh       # Runs at the start and middle of every hour
00 00,12 * * * bash /path/to/script.sh      # Runs every day at midnight and noon
00 * 01-07 01,06 * bash /path/to/script.sh  # Runs at the start of every hour for the first seven days of the months of January and June
```

- `scp` copies files between a cloud computer and your personal computer. Use the `-r` flag in order to copy directories.
- `curl` (client URL) allows you to send HTTP requests to other servers. Use the `-O` flag to download files with curl.
- `ps -A` lists all of the programs running in the background of your computer.
- `cron` allows you to schedule when programs are run. Use `crontab -e` in order to edit the cron table.
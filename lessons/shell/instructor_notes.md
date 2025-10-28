# 2025-11-04-strathclyde Shell lesson instructor notes

## Summary and Setup

**[SLIDE HERE: Links/QR codes]**

- Lesson site: [https://swcarpentry.github.io/shell-novice/](https://swcarpentry.github.io/shell-novice/)

### Summary

- The Unix shell has been around longer than most users, including me, have been alive
- It has survived and adapted, _unlike flares, glam rock, Madchester, and the comedy reputation of Friends_, because it is a **very powerful tool for controlling a computer**.
- With the shell you can carry out extremely powerful tasks, often with only a few keystrokes or lines of code.
- You can use it to **automate repetitive tasks** and **combine smaller tasks** into larger, even more powerful **workflows**.

- Importantly for us as scientists, it allows us to easily record what we did (**reproducibility**) and share the methodology (**repeatability**)

- If you want to use high-performance computing (such as ARCHIE-West, here at Strathclyde) in your work - and this is becoming much more necessary as the volumes of biological data increase - you will need to use the shell.

- We'll be working through the filesystem and the shell.
- If you have ever saved files on a computer and know the difference between a "file" and a "directory"/"folder", then you are ready for this lesson.

### Setup

- Please ensure that you have access to the Unix shell on your machine
  - University machine: please use Git bash (access via the Start menu)
  - Download/installation instructions: [https://carpentries.github.io/workshop-template/install_instructions/#shell](https://carpentries.github.io/workshop-template/install_instructions/#shell)

- You will need to download some files to follow this lesson
- **Please see the EtherPad page for a link**
  - [https://pad.carpentries.org/2025-10-04-strathclyde](https://pad.carpentries.org/2025-10-04-strathclyde)
  - Download this file to your `Desktop`
  - Unzip/uncompress the file
    - **NOTE: do not assume that your filesystem has unzipped the file; Windows will navigate into the compressed file using the explorer, without unzipping it!**

**[PAUSE: ensure that all learners have access to a bash shell]**

- Open a new instance of the shell on your computer

**[PAUSE: ensure that all learners can start a bash shell]**

### Introducing the shell

- There are many ways to interact with a computer. You'll probably be used to methods like:
  - using a **touchscreen**
  - **keyboard and mouse**
  - maybe even voice interaction/**speech recognition**
- The most common method, still, is via a **GUI** (Graphical User Interface), clicking with a mouse through a series of menu interactions

- The GUI approach is **easy to learn**, and it **may be intuitive**
- Unfortunately, **it does not scale well**. What do we mean by that?
  - Suppose you're carrying out a literature search.
  - Imagine you need to copy the affiliations for every author in 1000 papers to a single file, for analysis.
  - You could open each file, copy the affiliations, and paste them into an ever-growing file.
  - But this would take a long time.
  - It would also be prone to error - it's easy to **miss a character** when copy/pasting. You might even **miss an entire affiliation**, or **accidentally skip one or more files**. And **how would you know** if you'd done that? That would be a lot of finicky manual checking, and you'd possibly even **overlook some mistakes**.
- This is where the CLI (Command-Line Interface) approach is especially powerful.
  - These **repetitive tasks can be automated** for a single file, **and applied automatically across many files** - an arbitrary number - extremely quickly.
- You could spend hours manually copy/pasting and checking, or you could spend a minute writing a shell script, and get the job done in seconds.

#### The Shell

**[OPEN THE SHELL ON-SCREEN]**

- So **what is the shell**?
- The shell is **a program that runs on your computer**.
- Users, like you and me, can **type commands into the shell**, and the computer will run them.
- We can **use the shell to start other programs**, like modelling software for metabolism or industrial processes, protein structure prediction, or sequence analysis tools.
- The most common shell is called bash (Bourne-Again SHell) and is the default on many systems.
  - _On a Mac, you may see the Z Shell instead. This works just like bash._

- The big difference between a GUI and the shell is that **a GUI usually shows you what options/commands are available**.
  - **The shell does not**.
- It can take some time and effort to learn how to use the shell well.
  - **But you can get a long way with only a few commands - the ones we'll learn today.**

- In the shell, you can easily:
  - **combine existing tools and programs into pipelines and workflows**, for automation
  - **combine sequences of commands into _scripts_**, for reproducilty
  - **interact with powerful remote computing resources**, like ARCHIE-West (this is an increasingly important skill in modern biology)

#### Let's get started

- When you open the shell, you will see the **prompt**, which tells you the shell is waiting for your input:

```bash
(base) lpritc@Rodan-2 shell %
```

- **Your shell will look different to mine**.
  - The CLI prompt is customisable, and I have some extra information in my shell because it's useful for me in my day-to-day work.
  - The variation does not matter for the purpose of this lesson.
  - **The important part for this lesson is the `%` or `$` symbol**
  - You should also see a **cursor**, just after the prompt.
    - This might be a flashing block, a static block, an underscore, or something else. The visual appearance can vary depending on which system you are using.
  - If you can see this symbol, everything is fine.

- The shell is waiting for you to type a command.
- Once you have typed a command at the prompt, you execute it using the `enter` or `return` key.

- **Let's try our first command**L `ls`, short for "listing", which lists the current directory contents.
  - Type `ls` and press the `enter`/`return` key

```bash
(base) lpritc@Rodan-2 shell % ls
instructor_examples/ instructor_notes.md
(base) lpritc@Rodan-2 shell %
```

- Notice that once the command completes, you are presented with a new shell prompt, ready for the next command.

### Nelle's Pipeline

**[SLIDE HERE: Nelle's Pipeline details]**

- For the lesson we're going to pretend to be a marine biologist called Nelle
- Nelle, i.e. you, has/have just returned from a six-month long survey of the North Pacific Gyre, where you were sampling marine life in the Great Pacific Garbage Patch
  - You have **1520 samples** that you ran through a mass spec to gather the relative abundances of 300 different proteins
  - You need to **run these samples through an analysis tool** called `goostats.sh`
  - You also need to **write up your results by a deadline** at the end of the month.

- If you were to carry out this analysis by hand i**n a GUI, you'd have to select and open a file 1520 times**
  - If `goostats.sh` takes **30s to analyse a file, that's 12 hours of your time** just pointing and clicking.
  - **With the shell you can get the computer to handle all the boring stuff** while you crack on with the paper introduction.

- What you're going to do is:
  - **use the shell to run `goostats.sh`**
  - **use loops** to automate running `goostats.sh` on many different files
- As a bonus, **once this pipeline is up and running, you can use it again on new data**, whenever you collect it - saving you even more time.

- Specifically, you're going to:
  - navigate to a file/directory
  - create a file/directory
  - check the length of a file
  - chain commands together
  - retrieve a set of files
  - iterate over files
  - run a shell script containing your pipeline

#### Navigating files and directories: `ls`

- **[SLIDE HERE: Title Slide]**

- Let's start by seeing how to
  - **move around** on your computer
  - **see what files and directories** you have
  - **specify the location of a file or directory** on your computer

- **[SLIDES HERE: Filesystem Structure]**

- The part of the operating system responsible for managing files and directories is called **the file system**. It organizes our data into:
  - **files, which hold information**
  - **directories (also called ‘folders’), which hold files or other directories.**

- There are several commands for creating, inspecting, and deleting files and directories.
- Let’s find out where we are by running a command called pwd (which stands for ‘print working directory’).
  - Directories are like _places_
  - At any time while we are using the shell, we can be in exactly one place at any time (though we can move from one directory/place to another)
  - The place we are in is called our current working directory.
  - Commands mostly read and write files in the current working directory, i.e. ‘here’, so knowing where you are before running a command is important. 

**[OPEN SHELL IN HOME DIRECTORY]**

- The `pwd` command shows you where you are

```bash
(base) lpritc@Rodan-2 lpritc % pwd
/Users/lpritc
(base) lpritc@Rodan-2 lpritc %
```

- Here the response is `/Users/lpritc` - which is my **home directory** on this machine
  - On Linux, your home directory may look like `/home/nelle`, and on Windows, it might be similar to `C:\Documents and Settings\nelle` or `C:\Users\nelle` - this is normal and reflects the differences between operating systems

- **[RETURN TO SLIDES: Filesystem Structure]**

- Note that **your filesystem will not look exactly like that in the examples**, or like mine.
  - **You will sometimes need to modify the commands on screen so that they work on _your_ computer**.

- Let's look at Nelle's home directory (on the slide)
  - The filesystem on Nelle's machine is that shown on screen
  - Notice that it's a hierarchy that **looks like a family tree, or phylogenetic tree**.
  - **The very top directory is the _root directory_** and has the symbol `/` - this directory holds all of the other directories.
  - This directory is the leading slash in `/Users/lpritc`
- In Nelle's _root directory_ there are `bin`, `data`, `Users` and `tmp` directories.
  - We know that Nelle's working directory `/Users/nelle` is stored inside the `/Users` directory (this is the first part of the name returned by `pwd`)

- **[NEXT SLIDE: User Directories]**

- In the `/Users` directory, we find one directory for each user account on that machine
  - Nelle, Imhotep, and Larry
  - Nelle's home directory is `/Users/nelle` 
- **Generally, when you open a new shell, you start in your home directory**

- We can see what's in our home directory by using the `ls` command

- **[SHELL]**

```bash
(base) lpritc@Rodan-2 lpritc % ls
Applications/
Applications (Parallels)/
bin/
Calibre Library/
Desktop/
Documents/
Downloads/
Dropbox (Dropbox-Work)/
Google Drive@
iCloud Drive (Archive)/
iCloud Drive (Archive) - 1/
Library/
Movies/
Music/
nltk_data/
opt/
Parallels/
Pictures/
Public/
seaborn-data/
Zotero/
```

- **Your results will be different - this is normal**.
- My shell has colours that differentiate between directories and files - your shell might not do this by default.
  - If you'd like to see the colours, use `ls -F`:

```bash
% ls -F
```

#### Command-line options

- You'll see here that we have provided an **option** to `ls`, which tells it in more detail how it should present the contents of the directory
- Many commands have additional **options** and **arguments** that you can use to control their behaviour, and there are two standard ways to find out what these are, in the terminal
  - add the `--help` or `-h` option
  - use the `man` manual system

```bash
# ls --help doesn't work on macOS, but should on Linux and Git Bash
% ls --help
ls: unrecognized option `--help'
usage: ls [-@ABCFGHILOPRSTUWXabcdefghiklmnopqrstuvwxy1%,] [--color=when] [-D format] [file ...]
```

```bash
# man ls does work on macOS
% man ls
```

- You'll see that there are long and short options that are equivalent, like `-h` and `--help` - **it makes no difference to functionality which you use**
  - But, if you're **writing a script it can be more legible/reproducible to use the long form** of an option.

- **[SLIDE HERE: `ls` challenge]**
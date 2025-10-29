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

## Introducing the shell

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

### The Shell

**[OPEN THE SHELL ON-SCREEN]**

- So **what is the shell**?
- The shell is **a program that runs on your computer**.
- Users, like you and me, can **type commands into the shell**, and the computer will run them.
- We can **use the shell to start other programs**, like modelling software for metabolism or industrial processes, protein structure prediction, or sequence analysis tools.
- The most common shell is called bash (Bourne-Again SHell) and is the default on many systems.
  - _On a Mac, you may see the Z Shell (`zsh`) instead. This works just like bash._

- The big difference between a GUI and the shell is that **a GUI usually shows you what options/commands are available**.
  - **The shell does not**.
- It can take some time and effort to learn how to use the shell well.
  - **But you can get a long way with only a few commands - the ones we'll learn today.**

- In the shell, you can easily:
  - **combine existing tools and programs into pipelines and workflows**, for automation
  - **combine sequences of commands into _scripts_**, for reproducilty
  - **interact with powerful remote computing resources**, like ARCHIE-West (this is an increasingly important skill in modern biology)

### Let's get started

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

## Nelle's Pipeline

**[SLIDE HERE: Nelle's Pipeline outline]**

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

**[SLIDE HERE: Nelle's Pipeline details]**

- Specifically, you're going to:
  - navigate to a file/directory
  - create a file/directory
  - check the length of a file
  - chain commands together
  - retrieve a set of files
  - iterate over files
  - run a shell script containing your pipeline

## Navigating files and directories: `ls`

- **[SLIDE HERE: Title Slide]**

- Let's start by seeing how to
  - **move around** on your computer
  - **see what files and directories** you have
  - **specify the location of a file or directory** on your computer

- **[SLIDES HERE: Filesystem Structure]**

- The part of the operating system responsible for managing files and directories is called **the file system**. It organizes our data into:
  - **files, which hold information**
  - **directories (also called ‘folders’), which hold files or other directories.**

- Note that **your filesystem will not look exactly like that in the examples**, or like mine.
  - **You will sometimes need to modify the commands on screen so that they work on _your_ computer**.

- Let's look at Nelle's home directory (on the slide)
  - The filesystem on Nelle's machine is that shown on screen
  - Notice that it's a hierarchy that **looks like a family tree, or phylogenetic tree** - with the root at the top
  - **The very top directory is the _root directory_** and has the symbol `/` - this directory holds all of the other directories.
  - This directory is the leading slash in `/Users/lpritc`
- In Nelle's _root directory_ there are `bin`, `data`, `Users` and `tmp` directories.
  - We know that Nelle's working directory `/Users/nelle` is stored inside the `/Users` directory (this is the first part of the name returned by `pwd`)

- There are several commands for creating, inspecting, and deleting files and directories.
- Let’s find out where we are - ourselves - by running a command called pwd (which stands for ‘print working directory’).
  - Directories are like _places_
  - At any time while we are using the shell, we can be in exactly one place at any time (though we can move from one directory/place to another)
  - The place we are in is called our current working directory.
  - Commands mostly read and write files in the current working directory, i.e. ‘here’, so knowing where you are before running a command is important. 

- **[NEXT SLIDE: User Directories]**

- In the `/Users` directory, we find one directory for each user account on that machine (just like I have `lpritc` as my home directory)
  - Nelle, Imhotep, and Larry
  - Nelle's home directory is `/Users/nelle` 
- **Generally, when you open a new shell, you start in your home directory**

- We can see what's in our home directory by using the `ls` command

**[OPEN SHELL IN HOME DIRECTORY]**

- The `pwd` (print working directory) command in the shell shows you where you are

```bash
(base) lpritc@Rodan-2 lpritc % pwd
/Users/lpritc
(base) lpritc@Rodan-2 lpritc %
```
- Here the response is `/Users/lpritc` - which is my **home directory** on this machine
  - On Linux, your home directory may look like `/home/nelle`, and on Windows, it might be similar to `C:\Documents and Settings\nelle` or `C:\Users\nelle` - this is normal and reflects the differences between operating systems
- Let's see what's in the home directory:

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

### Command-line options

- You'll see here that we have provided an **option** to `ls`, which tells it in more detail how it should present the contents of the directory
  - _Options_ change the behaviour of commands.
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

- **[SLIDE HERE: `ls` challenge (reverse chronological)]**

### Exploring other directories

- We can use `ls` to list the contents of the current directory, and **we can also use it to list the contents of a different directory**
- Let's take a look at the contents of the desktop using `ls -F Desktop` - we tell `ls` the _path_ to the location we want to list contents for
  - This should show all the files and directories on your desktop (assuming you are in your home directory)
  - In amongst these should be the `shell-lesson-data` directory that you downloaded

- **[SHOW SHELL]**

```bash
% ls -F Desktop
[...]
shell-lesson-data/
[...]
```

- We can use the same strategy to look _inside_ the `shell-lesson-data` directory, with `ls -F Desktop/shell-lesson-data`
  - We see the two directories: `exercise-data` and `north-pacific-gyre`

```bash
% ls -F Desktop/shell-lesson-data
exercise-data/      north-pacific-gyre/
```

- We are currently located in our home directories, but we want to be in the `exercise-data` directory, so we can progress with the lesson.
- To do this we use the `cd` (change directory) command, telling it which directory we want to move to
  - We can do this three times, stepping first into `Desktop`, then `shell-lesson-data`, and then to `exercise-data` confirm the move with `pwd`:

```bash
% cd Desktop
% cd shell-lesson-data
% cd exercise-data
% pwd
/Users/lpritc/Desktop/shell-lesson-data/exercise-data
```

- Now we can see the contents of the folder with `ls -F`

```bash
% ls -F                                                                                                                                                  [1:33:18]
alkanes/       animal-counts/ creatures/     numbers.txt    writing/
```

- We've moved _down_ in the directory tree, but **how do we move up?**
  - We can't use `cd shell-lesson-data` because we can only "see" the current contents of the directory
  - But there is **a special shortcut to move up one directory level**: `..`

```bash
% cd ..
% pwd
/Users/lpritc/Desktop/shell-lesson-data
```

### Other shortcuts

- We can always return to our home directory using the shortcut `~`, which stands for "home directory"

```bash
% cd ~
% pwd
/Users/lpritc
```

- You can `cd` directly to a location by using its "absolute path" (starting with `/` at the root)

```bash
% cd /Users/lpritc/Desktop/shell-lesson-data
% pwd
/Users/lpritc/Desktop/shell-lesson-data
```

- And you can use `~` to indicate your home directory as a starting point

```bash
% cd ~/Desktop/shell-lesson-data/exercise-data
% pwd
/Users/lpritc/Desktop/shell-lesson-data/exercise-data
```

- If you use `ls -a` - the option to list all files and directories, you will see two directory shortcuts:

```bash
% ls -a
./             ../            alkanes/       animal-counts/ creatures/     numbers.txt    writing/
```

- You've seen `..` which means "the directory above this one"
  - The `.` shortcut means "this current directory

```bash
% cd .
% pwd
/Users/lpritc/Desktop/shell-lesson-data/exercise-data
```

**[SLIDE HERE: Challenge (absolute vs relative paths)]**

**[SLIDE HERE: Challenge (relative path resolution)]**

## Syntax of shell commands

- Now that we know something about the `ls` command, and how to move around the filesystem, let's talk about how the commands are put together
  - Let's think about the command `ls -F /`, which lists the contents of the filesystem's root directory.

**[SLIDE HERE: shell command syntax]**

- `ls` is the _command_ - this tells the computer what program runs, or what action to take
- `-F` is an _option_ - this changes the behaviour of the command in a particular way
  - options can start with a single dash `-` (short option) or double-dash `--` (long option)
- `/` is an _argument_ - it tells the command what it should operate on (usually a file or directory)

- **Commands may take zero, one, or more arguments, and zero, one or more options**

- Each part of the command is separated by a space character.
  - If you miss out the space between `ls` and `-F`, the computer would look for a command called `ls-F`, which doesn't exist
  - **This is why you should never use spaces in filenames** - the computer will misinterpret them

- Note that upper and lower case are different, and this matters

**[SHOW SHELL]**

```bash
% ls -s  # displays the size of a file/directory
total 8
0 alkanes/       0 animal-counts/ 0 creatures/     8 numbers.txt    0 writing/
% ls -S  # sorts files/directories by size
alkanes/       creatures/     writing/       animal-counts/ numbers.txt
```

## Working With Files and Directories

- We know how to explore files and directories, but how do we _create_ them?
- Before we start, let's make sure we're in the `shell-lesson-data` directory

**NAVIGATE TO `shell-lesson-data`**

```bash
% pwd
/Users/lpritc/Desktop/shell-lesson-data
```

- Now we'll navigate to the `exercise/writing` directory and see what's in it

```bash
% cd exercise-data/writing
% ls -F
haiku.txt        LittleWomen.txt
```

### Creating directories

- Nelle wants to write her thesis, so let's create a directory to write in
- The command for this is `mkdir` (make directory)
- We can confirm that this created using `ls -F`, and also that there's nothing in it to begin with

```bash
% mkdir thesis
% ls -F
haiku.txt        LittleWomen.txt  thesis/
% ls -F thesis
```

- We don't have to create only one directory at a time, and we don't have to create a directory only in our current location
- With the `-p` option to mkdir we can create nested subdirectories in a neighbouring directory called `project`
- We can then check the contents of `project` with `ls -FR`

```bash
% mkdir -p ../project/data ../project/results
% ls -FR ../project
data/    results/

../project/data:

../project/results:
```

**[SHOW SLIDE: Names for files and directories]**

- Use descriptive filenames so that you don't need to look inside a file to know what it contains
- Stick to lower-case letters, numbers, and these special characters (`,` `-` `_`) so you avoid clashes with some special instructions
  - Some filesystems can be case-insensitive so don't distinguish between `thesis` and `Thesis`
- Don't use spaces
- Don't start a name with a dash/hyphen

### Create a text file

- Let's create a text file

**[SHOW SHELL]**

- We'll move our _working directory_ to `thesis` and then run a text editor called `Nano` to create a file called `draft.txt`

```bash
% cd thesis
% nano draft.txt
```

- On my machine this starts an editor called `Pico` - but it's essentially the same thing
- We can type in a couple of lines of text

```text
It's not "publish or perish" any more,
it's "share and thrive"
```

- To save the text, we use the `WriteOut` command in `Nano`
  - Hold down the `Ctrl` key, and press `O`
  - We're asked what file name to write the text to - and it's suggesting `draft.txt`
  - Press `Return` to accept this default name
- Once the file is saved, use the `Exit` command to return to the shell
  - Hold down the `Ctrl` key, and press `X`

- We can see that we've saved a file, with `ls`

```bash
% ls
draft.txt
```

**[SHOW SLIDE: File extensions]**

- You'll probably have noticed that all of Nelle's files - and most files in general - are called "something dot something"
  - This is just a conventions, we can actually use any name we like
- By convention, file extensions indicate the type of data in the file
  - `.txt` indicates a plain text file
  - `.png` indicates a PNG image
- But this is **only a convention** - the extension might be misleading
  - **And you cannot change file content type by changing the extension**
  
### Moving files and directories

- Let's go back to the `writing` directory

```bash
% pwd
/Users/lpritc/Desktop/shell-lesson-data/exercise-data/writing/thesis
% cd ..
```

- We might consider that the file we just created has an inappropriate name
  - `draft` is ambiguous, and as it's a collection of quotes, `quotes.txt` might be better
  - So we want to rename `draft.txt` to `quotes.txt`
- To rename a file, we actually need to **move** it, with the command `mv` (short for "move")
  - We need to say what file we're moving, and where we're moving it to

```bash
% mv thesis/draft.txt thesis/quotes.txt
% ls thesis
quotes.txt
```

- We have to be careful when doing this because `mv` will **silently** overwrite the target file, if it exists
- Suppose we decide that we want to put the `quotes.txt` file in the current director?
  - We use the `mv` (move) command again, but specify the directory we want to move the file to

```bash
% mv thesis/quotes.txt .
% ls thesis
% ls
haiku.txt        LittleWomen.txt  quotes.txt       thesis/
```

## Copying files and directories

- The `cp` command (short for "copy") works just like the `mv` command, except it _copies_ a file rather than moving it
  - Let's copy the `quotes.txt` file to `thesis/quotations.txt`

```bash
% cp quotes.txt thesis/quotations.txt
% ls quotes.txt thesis/quotations.txt
quotes.txt             thesis/quotations.txt
```

- To copy an entire directory and its contents we need to use the `-r` argument (which stands for "recursive")
  - To back up the thesis directory

```bash
% cp -r thesis thesis_backup
% ls thesis thesis_backup
thesis:
quotations.txt

thesis_backup:
quotations.txt
```

**[SLIDE HERE: Challenge (renaming files)]**

## Removing files and directories

**[SHOW SHELL]**

- We like the `quotations.txt` file being where it is, so we want to get rid of the redundant `quotes.txt` file
- To do this, we use the `rm` (remove) command

```bash
% rm quotes.txt
% ls quotes.txt
ls: quotes.txt: No such file or directory
```

- We need to be careful when using `rm`: **There is no trash bin or recovery of deleted files - they're gone forever**
- We can introduce a kind of protection by using the `-i` ("interactive") option, which asks us if we really want to delete a file

```bash
% rm -i thesis/quotations.txt
remove thesis/quotations.txt? n
```

- We cannot remove directories using `rm` unless we use the `-r` (recursive) option

```bash
% rm thesis
rm: thesis: is a directory
```

## Operations with multiple files and directories

- You've already seen how to give a command multiple filenames, with things like

```bash
% ls haiku.txt thesis/quotations.txt
haiku.txt              thesis/quotations.txt
```

- Let's move up a directory level to `exercise-data` and look into the `creatures` directory

```bash
% cd ..
% pwd
/Users/lpritc/Desktop/shell-lesson-data/exercise-data
% ls creatures
basilisk.dat  minotaur.dat  unicorn.dat
```

- We're going to try to copy two of those `.dat` files to a new directory called `backup`

```bash
% mkdir backup
% cp creatures/minotaur.dat creatures/unicorn.dat backup/
% ls backup
minotaur.dat  unicorn.dat
```

- It's a bit of a pain to write out the filenames in full, so we use **wildcards** - special symbols that represent other characters.
- Let's look at the `alkanes` directory

```bash
% ls alkanes
cubane.pdb   ethane.pdb   methane.pdb  octane.pdb   pentane.pdb  propane.pdb
```

- It has six files in it, all ending in `.pdb`
- We can use the `*` wildcard to representa any sequence of characters - **including an empty string**
  - The string `*.pdb` represents "any filename ending in `.pdb`
  - But the string `p*.pdb` represents "any filename starting with `p` and ending with `.pdb`

```bash
% ls alkanes/*.pdb
alkanes/cubane.pdb   alkanes/ethane.pdb   alkanes/methane.pdb  alkanes/octane.pdb   alkanes/pentane.pdb  alkanes/propane.pdb
% ls alkanes/p*.pdb
alkanes/pentane.pdb  alkanes/propane.pdb
```

- The `?` wildcard represents **a single character**

```bash
% ls alkanes/?ethane.pdb
alkanes/methane.pdb
% ls alkanes/*ethane.pdb
alkanes/ethane.pdb   alkanes/methane.pdb
% ls alkanes/???ane.pdb
alkanes/cubane.pdb  alkanes/ethane.pdb  alkanes/octane.pdb
```

**[SHOW SLIDES: Challenge (pattern matching)]**

## Pipes and Filters

- Now you know how to move around and manipulate the filesystem, we can look at **how to combine existing programs in new ways**
- We'll do this in the `alkanes` directory

**[SHOW SHELL]**

```bash
% cd alkanes
% ls
cubane.pdb   ethane.pdb   methane.pdb  octane.pdb   pentane.pdb  propane.pdb
```

- Let's run an example command: `wc` ("word count")

```bash
% wc cubane.pdb
      20     156    1158 cubane.pdb
```

- This reports the number of _lines_ (20), _words_ (156), and _characters_ (1158) in a file.
- You can use the `*` wildcard to see this information for all files in the directory

```bash
% wc *.pdb
      20     156    1158 cubane.pdb
      12      84     622 ethane.pdb
       9      57     422 methane.pdb
      30     246    1828 octane.pdb
      21     165    1226 pentane.pdb
      15     111     825 propane.pdb
     107     819    6081 total
```

- We can change the behaviour of `wc` with an option like `-l` to report only the number of lines

```bash
% wc -l *.pdb
      20 cubane.pdb
      12 ethane.pdb
       9 methane.pdb
      30 octane.pdb
      21 pentane.pdb
      15 propane.pdb
     107 total
```

- Suppose we wanted to know which file was the shortest, in that it contained the fewest lines of text?
- We can see easily enough here with six files (it's `methane.pdb`), but with 6000 that would be more difficult
- **Let's build a tool to do this for us**

### Capturing output from commands

- We'll start by **redirecting** the output of `wc` to a file, using the `>` redirection symbol
  - This shows no output on screen because we redirect it to a file

```bash
% wc -l *.pdb > lengths.txt
% ls lengths.txt
lengths.txt
```

- We can inspect the contents of a file using the `cat` (concatenate) command

```bash
% cat lengths.txt
      20 cubane.pdb
      12 ethane.pdb
       9 methane.pdb
      30 octane.pdb
      21 pentane.pdb
      15 propane.pdb
     107 total
```

### Filtering output

- There is a command called `sort` that sorts the contents of a file
- We can test this out with the `numbers.txt` file in the directory above `alkanes`

```bash
% cat ../numbers.txt
10
2
19
22
6
% sort ../numbers.txt
10
19
2
22
6
```

- Why do you think the output looks like this?
  - Unless told otherwise, `sort` treats all file data as alphanumerical character strings - i.e. it treats numbers like letters
- To sort numerical data in numerical order, we need to use the `-n` option with `sort`

```bash
% sort -n ../numbers.txt
2
6
10
19
22
```

- Let's do this with our `lengths.txt` file:

```bash
% sort -n lengths.txt
       9 methane.pdb
      12 ethane.pdb
      15 propane.pdb
      20 cubane.pdb
      21 pentane.pdb
      30 octane.pdb
     107 total
```

- We can put the sorted list into a new temporary file called `sorted-lengths.txt` using **redirection**
  - Then we can use the `head` command to see only the first line of the file, which gives us the name of the shortest file
  - The `-n 1` option tells `head` that we only want to see one line from the file (`-n 5` would show the first five lines)

```bash
% sort -n lengths.txt > sorted-lengths.txt
% head -n 1 sorted-lengths.txt
       9 methane.pdb
```

### Passing output to another command

- To do this so far, we've had to use two intermediate files: `lengths.txt` and `sorted-lengths.txt`
  - This is an inefficient way to work, and would quickly fill up our filesystem with intermediate files that we'd never use again
- Instead of writing intermediate files, we can **pipe** commands together so that the output of one command becomes the input to the next
  - We do this with the **pipe symbol** `|`
- For instance, to `sort` the contents of `lengths.txt` and use the sorted output as the input to `head -n 1`:

```bash
% sort -n lengths.txt | head -n 1
       9 methane.pdb
```

- Similarly, we could count the words in all our `.pdb` files and pipe this to `sort`:

```bash
% wc -l *.pdb | sort -n
       9 methane.pdb
      12 ethane.pdb
      15 propane.pdb
      20 cubane.pdb
      21 pentane.pdb
      30 octane.pdb
     107 total
```

### Combining multiple commands

- The magic starts to happen when we realise we don't need to restrict ourselves to only two commands.
- We can chain multiple commands together using pipes:

```bash
% wc -l *.pdb | sort -n | head -n 1
       9 methane.pdb
```

**[SHOW SLIDES: Combining multiple commands AND challenge]***

- This way of linking programs together is one reason why Unix is so successful
  - We don't need to make huge programs that do lots of slightly different things
  - Instead we make tools that do one job well, but that can link well with other small programs
- **You can, and should, write your programs so that they play nicely with existing Unix tools**

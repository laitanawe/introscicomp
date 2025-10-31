---
title: "Introducing the Shell"
teaching: 60
exercises: 30
questions:
- "What is a command shell and why would I use one?"
objectives:
- "Explain how the shell relates to the keyboard, the screen, the operating system, and users' programs."
- "Explain when and why command-line interfaces should be used instead of graphical interfaces."
keypoints:
- "A shell is a program whose primary purpose is to read commands and run other programs."
-  "This lesson uses Bash, the default shell in many implementations of Unix."
-  "Programs can be run in Bash by entering commands at the command-line prompt."
- "The shell's main advantages are its high action-to-keystroke ratio, its support for
automating repetitive tasks, and its capacity to access networked machines."
- "The shell's main disadvantages are its primarily textual nature and how
cryptic its commands and operation can be."
---
### Background

Humans and computers commonly interact in many different ways, such as through a keyboard and mouse,
touch screen interfaces, or using speech recognition systems.
The most widely used way to interact with personal computers is called a
**graphical user interface** (GUI).
With a GUI, we give instructions by clicking a mouse and using menu-driven interactions.

While the visual aid of a GUI makes it intuitive to learn,
this way of delivering instructions to a computer scales very poorly.
Imagine the following task:
for a literature search, you have to copy the third line of one thousand text files in one thousand
different directories and paste it into a single file.
Using a GUI, you would not only be clicking at your desk for several hours,
but you could potentially also commit an error in the process of completing this repetitive task.
This is where we take advantage of the Unix shell.
The Unix shell is both a **command-line interface** (CLI) and a scripting language,
allowing such repetitive tasks to be done automatically and fast.
With the proper commands, the shell can repeat tasks with or without some modification
as many times as we want.
Using the shell, the task in the literature example can be accomplished in seconds.


### The Shell


The shell is a program where users can type commands.
With the shell, it's possible to invoke complicated programs like climate modeling software
or simple commands that create an empty directory with only one line of code.
The most popular Unix shell is Bash (the Bourne Again SHell ---
so-called because it's derived from a shell written by Stephen Bourne).
Bash is the default shell on most modern implementations of Unix and in most packages that provide
Unix-like tools for Windows.

Using the shell will take some effort and some time to learn.
While a GUI presents you with choices to select, CLI choices are not automatically presented to you,
so you must learn a few commands like new vocabulary in a language you're studying.
However, unlike a spoken language, a small number of "words" (i.e. commands) gets you a long way,
and we'll cover those essential few today.

The grammar of a shell allows you to combine existing tools into powerful
pipelines and handle large volumes of data automatically. Sequences of
commands can be written into a *script*, improving the reproducibility of
workflows.

In addition, the command line is often the easiest way to interact with remote machines
and supercomputers.
Familiarity with the shell is near essential to run a variety of specialized tools and resources
including high-performance computing systems.
As clusters and cloud computing systems become more popular for scientific data crunching,
being able to interact with the shell is becoming a necessary skill.
We can build on the command-line skills covered here
to tackle a wide range of scientific questions and computational challenges.

Let's get started.

When the shell is first opened, you are presented with a **prompt**,
indicating that the shell is waiting for input.

~~~
$
~~~
{: .language-bash}

The shell typically uses `$ ` as the prompt, but may use a different symbol.
In the examples for this lesson, we'll show the prompt as `$ `.
Most importantly:
when typing commands, either from these lessons or from other sources,
*do not type the prompt*, only the commands that follow it.
Also note that after you type a command, you have to press the <kbd>Enter</kbd> key to execute it.

The prompt is followed by a **text cursor**, a character that indicates the position where your
typing will appear.
The cursor is usually a flashing or solid block, but it can also be an underscore or a pipe.
You may have seen it in a text editor program, for example.

So let's try our first command, `ls` which is short for listing.
This command will list the contents of the current directory:

~~~
$ ls
~~~
{: .language-bash}

~~~
Desktop     Downloads   Movies      Pictures
Documents   Library     Music       Public
~~~
{: .output}

~~~
$ pwd
~~~
{: .language-bash}

~~~
$ ls -F
~~~
{: .language-bash}



### Getting help

`ls` has lots of other **options**. There are two common ways to find out how
to use a command and what options it accepts ---
**depending on your environment, you might find that only one of these ways works:**

1. We can pass a `--help` option to the command (not available on macOS), such as:
    ~~~
    $ ls --help
    ~~~
    {: .language-bash}

2. We can read its manual with `man` (not available in Git Bash), such as:
    ~~~
    $ man ls
    ~~~
    {: .language-bash}

We'll describe both ways next.

#### The `--help` option

Most bash commands and programs that people have written to be
run from within bash, support a `--help` option that displays more
information on how to use the command or program.

~~~
$ ls --help
~~~
{: .language-bash}

~~~
Usage: ls [OPTION]... [FILE]...
List information about the FILEs (the current directory by default).
Sort entries alphabetically if neither -cftuvSUX nor --sort is specified.

Mandatory arguments to long options are mandatory for short options, too.
  -a, --all                  do not ignore entries starting with .
  -A, --almost-all           do not list implied . and ..
      --author               with -l, print the author of each file
  -b, --escape               print C-style escapes for nongraphic characters
      --block-size=SIZE      scale sizes by SIZE before printing them; e.g.,
                               '--block-size=M' prints sizes in units of
                               1,048,576 bytes; see SIZE format below
  -B, --ignore-backups       do not list implied entries ending with ~
  -c                         with -lt: sort by, and show, ctime (time of last
                               modification of file status information);
                               with -l: show ctime and sort by name;
                               otherwise: sort by ctime, newest first
  -C                         list entries by columns
      --color[=WHEN]         colorize the output; WHEN can be 'always' (default
                               if omitted), 'auto', or 'never'; more info below
  -d, --directory            list directories themselves, not their contents
  -D, --dired                generate output designed for Emacs' dired mode
  -f                         do not sort, enable -aU, disable -ls --color
  -F, --classify             append indicator (one of */=>@|) to entries
...        ...        ...
~~~
{: .output}


#### The `man` command

The other way to learn about `ls` is to type
~~~
$ man ls
~~~
{: .language-bash}

This command will turn your terminal into a page with a description
of the `ls` command and its options.

To navigate through the `man` pages,
you may use <kbd>↑</kbd> and <kbd>↓</kbd> to move line-by-line,
or try <kbd>B</kbd> and <kbd>Spacebar</kbd> to skip up and down by a full page.
To search for a character or word in the `man` pages,
use <kbd>/</kbd> followed by the character or word you are searching for.
Sometimes a search will result in multiple hits.
If so, you can move between hits using <kbd>N</kbd> (for moving forward) and
<kbd>Shift</kbd>+<kbd>N</kbd> (for moving backward).

To **quit** the `man` pages, press <kbd>Q</kbd>.

~~~
-bash: cd: shell-lesson-data: No such file or directory
~~~
{: .error}

But we get an error! Why is this?

With our methods so far,
`cd` can only see sub-directories inside your current directory. There are
different ways to see directories above your current location; we'll start
with the simplest.

There is a shortcut in the shell to move up one directory level
that looks like this:

~~~
$ cd ..
~~~
{: .language-bash}

`..` is a special directory name meaning
"the directory containing this one",
or more succinctly,
the **parent** of the current directory.
Sure enough,
if we run `pwd` after running `cd ..`, we're back in `/Users/nelle/Desktop/shell-lesson-data`:

~~~
$ pwd
~~~
{: .language-bash}

~~~
/Users/nelle/Desktop/shell-lesson-data
~~~
{: .output}

The special directory `..` doesn't usually show up when we run `ls`. If we want
to display it, we can add the `-a` option to `ls -F`:

~~~
$ ls -F -a
~~~
{: .language-bash}

~~~
./  ../  exercise-data/  north-pacific-gyre/
~~~
{: .output}

`-a` stands for 'show all';
it forces `ls` to show us file and directory names that begin with `.`,
such as `..` (which, if we're in `/Users/nelle`, refers to the `/Users` directory).
As you can see,
it also displays another special directory that's just called `.`,
which means 'the current working directory'.
It may seem redundant to have a name for it,
but we'll see some uses for it soon.

Note that in most command line tools, multiple options can be combined
with a single `-` and no spaces between the options: `ls -F -a` is
equivalent to `ls -Fa`.

> ## Other Hidden Files
>
> In addition to the hidden directories `..` and `.`, you may also see a file
> called `.bash_profile`. This file usually contains shell configuration
> settings. You may also see other files and directories beginning
> with `.`. These are usually files and directories that are used to configure
> different programs on your computer. The prefix `.` is used to prevent these
> configuration files from cluttering the terminal when a standard `ls` command
> is used.
{: .callout}

These three commands are the basic commands for navigating the filesystem on your computer:
`pwd`, `ls`, and `cd`. Let's explore some variations on those commands. What happens
if you type `cd` on its own, without giving
a directory?

~~~
$ cd
~~~
{: .language-bash}

How can you check what happened? `pwd` gives us the answer!

~~~
$ pwd
~~~
{: .language-bash}

~~~
/Users/nelle
~~~
{: .output}

It turns out that `cd` without an argument will return you to your home directory,
which is great if you've got lost in your own filesystem.

Let's try returning to the `exercise-data` directory from before. Last time, we used
three commands, but we can actually string together the list of directories
to move to `exercise-data` in one step:

~~~
$ cd Desktop/shell-lesson-data/exercise-data
~~~
{: .language-bash}

Check that we've moved to the right place by running `pwd` and `ls -F`.

If we want to move up one level from the data directory, we could use `cd ..`.  But
there is another way to move to any directory, regardless of your
current location.

So far, when specifying directory names, or even a directory path (as above),
we have been using **relative paths**.  When you use a relative path with a command
like `ls` or `cd`, it tries to find that location from where we are,
rather than from the root of the file system.

However, it is possible to specify the **absolute path** to a directory by
including its entire path from the root directory, which is indicated by a
leading slash. The leading `/` tells the computer to follow the path from
the root of the file system, so it always refers to exactly one directory,
no matter where we are when we run the command.

This allows us to move to our `shell-lesson-data` directory from anywhere on
the filesystem (including from inside `exercise-data`). To find the absolute path
we're looking for, we can use `pwd` and then extract the piece we need
to move to `shell-lesson-data`.

~~~
$ pwd
~~~
{: .language-bash}
> ## Command not found
> If the shell can't find a program whose name is the command you typed, it
> will print an error message such as:
>
> ~~~
> $ ks
> ~~~
> {: .language-bash}
> ~~~
> ks: command not found
> ~~~
> {: .output}
>
> This might happen if the command was mis-typed or if the program corresponding to that command
> is not installed.
{: .callout}


## Nelle's Pipeline: A Typical Problem

Nelle Nemo, a marine biologist,
has just returned from a six-month survey of the
[North Pacific Gyre](http://en.wikipedia.org/wiki/North_Pacific_Gyre),
where she has been sampling gelatinous marine life in the
[Great Pacific Garbage Patch](http://en.wikipedia.org/wiki/Great_Pacific_Garbage_Patch).
She has 1520 samples that she's run through an assay machine to measure the relative abundance
of 300 proteins.
She needs to run these 1520 files through an imaginary program called `goostats.sh` she inherited.
On top of this huge task, she has to write up results by the end of the month so her paper
can appear in a special issue of *Aquatic Goo Letters*.

The bad news is that if she has to run `goostats.sh` by hand using a GUI,
she'll have to select and open a file 1520 times.
If `goostats.sh` takes 30 seconds to run each file, the whole process will take more than 12 hours
of Nelle's attention.
With the shell, Nelle can instead assign her computer this mundane task while she focuses
her attention on writing her paper.

The next few lessons will explore the ways Nelle can achieve this.
More specifically,
they explain how she can use a command shell to run the `goostats.sh` program,
using loops to automate the repetitive steps of entering file names,
so that her computer can work while she writes her paper.

As a bonus,
once she has put a processing pipeline together,
she will be able to use it again whenever she collects more data.

In order to achieve her task, Nelle needs to know how to:
- navigate to a file/directory
- create a file/directory
- check the length of a file
- chain commands together
- retrieve a set of files
- iterate over files
- run a shell script containing her pipeline



## Exercies

> ## Absolute vs Relative Paths
>
> Starting from `/Users/amanda/data`,
> which of the following commands could Amanda use to navigate to her home directory,
> which is `/Users/amanda`?
>
> 1. `cd .`
> 2. `cd /`
> 3. `cd /home/amanda`
> 4. `cd ../..`
> 5. `cd ~`
> 6. `cd home`
> 7. `cd ~/data/..`
> 8. `cd`
> 9. `cd ..`
>
> > ## Solution
> > 1. No: `.` stands for the current directory.
> > 2. No: `/` stands for the root directory.
> > 3. No: Amanda's home directory is `/Users/amanda`.
> > 4. No: this command goes up two levels, i.e. ends in `/Users`.
> > 5. Yes: `~` stands for the user's home directory, in this case `/Users/amanda`.
> > 6. No: this command would navigate into a directory `home` in the current directory
> >     if it exists.
> > 7. Yes: unnecessarily complicated, but correct.
> > 8. Yes: shortcut to go back to the user's home directory.
> > 9. Yes: goes up one level.
> {: .solution}
{: .challenge}

> ## Relative Path Resolution
>
> Using the filesystem diagram below, if `pwd` displays `/Users/thing`,
> what will `ls -F ../backup` display?
>
> 1.  `../backup: No such file or directory`
> 2.  `2012-12-01 2013-01-08 2013-01-27`
> 3.  `2012-12-01/ 2013-01-08/ 2013-01-27/`
> 4.  `original/ pnas_final/ pnas_sub/`
>
> ![A directory tree below the Users directory where "/Users" contains the
directories "backup" and "thing"; "/Users/backup" contains "original",
"pnas_final" and "pnas_sub"; "/Users/thing" contains "backup"; and
"/Users/thing/backup" contains "2012-12-01", "2013-01-08" and
"2013-01-27"](../fig/filesystem-challenge.svg)
>
> > ## Solution
> > 1. No: there *is* a directory `backup` in `/Users`.
> > 2. No: this is the content of `Users/thing/backup`,
> >    but with `..`, we asked for one level further up.
> > 3. No: see previous explanation.
> > 4. Yes: `../backup/` refers to `/Users/backup/`.
> {: .solution}
{: .challenge}

> ## `ls` Reading Comprehension
>
> Using the filesystem diagram below,
> if `pwd` displays `/Users/backup`,
> and `-r` tells `ls` to display things in reverse order,
> what command(s) will result in the following output:
>
> ~~~
> pnas_sub/ pnas_final/ original/
> ~~~
> {: .output}
>
> ![A directory tree below the Users directory where "/Users" contains the
directories "backup" and "thing"; "/Users/backup" contains "original",
"pnas_final" and "pnas_sub"; "/Users/thing" contains "backup"; and
"/Users/thing/backup" contains "2012-12-01", "2013-01-08" and
"2013-01-27"](../fig/filesystem-challenge.svg)
>
> 1.  `ls pwd`
> 2.  `ls -r -F`
> 3.  `ls -r -F /Users/backup`
>
> > ## Solution
> >  1. No: `pwd` is not the name of a directory.
> >  2. Yes: `ls` without directory argument lists files and directories
> >     in the current directory.
> >  3. Yes: uses the absolute path explicitly.
> {: .solution}
{: .challenge}

## Copying files and directories

The `cp` command works very much like `mv`,
except it copies a file instead of moving it.
We can check that it did the right thing using `ls`
with two paths as arguments --- like most Unix commands,
`ls` can be given multiple paths at once:

~~~
$ cp quotes.txt thesis/quotations.txt
$ ls quotes.txt thesis/quotations.txt
~~~
{: .language-bash}

~~~
quotes.txt   thesis/quotations.txt
~~~
{: .output}

We can also copy a directory and all its contents by using the
[recursive](https://en.wikipedia.org/wiki/Recursion) option `-r`,
e.g. to back up a directory:

```
$ cp -r thesis thesis_backup
```
{: .language-bash}

We can check the result by listing the contents of both the `thesis` and `thesis_backup` directory:

```
$ ls thesis thesis_backup
```
{: .language-bash}

```
thesis:
quotations.txt

thesis_backup:
quotations.txt
```
{: .output}


> ## Renaming Files
>
> Suppose that you created a plain-text file in your current directory to contain a list of the
> statistical tests you will need to do to analyze your data, and named it: `statstics.txt`
>
> After creating and saving this file you realize you misspelled the filename! You want to
> correct the mistake, which of the following commands could you use to do so?
>
> 1. `cp statstics.txt statistics.txt`
> 2. `mv statstics.txt statistics.txt`
> 3. `mv statstics.txt .`
> 4. `cp statstics.txt .`
>
> > ## Solution
> > 1. No.  While this would create a file with the correct name,
> > the incorrectly named file still exists in the directory
> > and would need to be deleted.
> > 2. Yes, this would work to rename the file.
> > 3. No, the period(.) indicates where to move the file, but does not provide a new file name;
> > identical file names
> > cannot be created.
> > 4. No, the period(.) indicates where to copy the file, but does not provide a new file name;
> > identical file names cannot be created.
> {: .solution}
{: .challenge}

> ## Moving and Copying
>
> What is the output of the closing `ls` command in the sequence shown below?
>
> ~~~
> $ pwd
> ~~~
> {: .language-bash}
> ~~~
> /Users/jamie/data
> ~~~
> {: .output}
> ~~~
> $ ls
> ~~~
> {: .language-bash}
> ~~~
> proteins.dat
> ~~~
> {: .output}
> ~~~
> $ mkdir recombined
> $ mv proteins.dat recombined/
> $ cp recombined/proteins.dat ../proteins-saved.dat
> $ ls
> ~~~
> {: .language-bash}
>
>
> 1.   `proteins-saved.dat recombined`
> 2.   `recombined`
> 3.   `proteins.dat recombined`
> 4.   `proteins-saved.dat`
>
> > ## Solution
> > We start in the `/Users/jamie/data` directory, and create a new folder called `recombined`.
> > The second line moves (`mv`) the file `proteins.dat` to the new folder (`recombined`).
> > The third line makes a copy of the file we just moved.
> > The tricky part here is where the file was copied to.
> > Recall that `..` means 'go up a level', so the copied file is now in `/Users/jamie`.
> > Notice that `..` is interpreted with respect to the current working
> > directory, **not** with respect to the location of the file being copied.
> > So, the only thing that will show using ls (in `/Users/jamie/data`) is the recombined folder.
> >
> > 1. No, see explanation above.  `proteins-saved.dat` is located at `/Users/jamie`
> > 2. Yes
> > 3. No, see explanation above.  `proteins.dat` is located at `/Users/jamie/data/recombined`
> > 4. No, see explanation above.  `proteins-saved.dat` is located at `/Users/jamie`
> {: .solution}
{: .challenge}

## Removing files and directories

Returning to the `shell-lesson-data/exercise-data/writing` directory,
let's tidy up this directory by removing the `quotes.txt` file we created.
The Unix command we'll use for this is `rm` (short for 'remove'):

~~~
$ rm quotes.txt
~~~
{: .language-bash}

We can confirm the file has gone using `ls`:

~~~
$ ls quotes.txt
~~~
{: .language-bash}

```
ls: cannot access 'quotes.txt': No such file or directory
```
{: .error}

> ## Deleting Is Forever
>
> The Unix shell doesn't have a trash bin that we can recover deleted
> files from (though most graphical interfaces to Unix do).  Instead,
> when we delete files, they are unlinked from the file system so that
> their storage space on disk can be recycled. Tools for finding and
> recovering deleted files do exist, but there's no guarantee they'll
> work in any particular situation, since the computer may recycle the
> file's disk space right away.
{: .callout}


> ## Using `rm` Safely
>
> What happens when we execute `rm -i thesis_backup/quotations.txt`?
> Why would we want this protection when using `rm`?
>
> > ## Solution
> > ```
> > rm: remove regular file 'thesis_backup/quotations.txt'? y
> > ```
> > {: .output}
> > The `-i` option will prompt before (every) removal (use <kbd>Y</kbd> to confirm deletion
> > or <kbd>N</kbd> to keep the file).
> > The Unix shell doesn't have a trash bin, so all the files removed will disappear forever.
> > By using the `-i` option, we have the chance to check that we are deleting only the files
> > that we want to remove.
> {: .solution}
{: .challenge}


If we try to remove the `thesis` directory using `rm thesis`,
we get an error message:

~~~
$ rm thesis
~~~
{: .language-bash}

~~~
rm: cannot remove `thesis': Is a directory
~~~
{: .error}

This happens because `rm` by default only works on files, not directories.

`rm` can remove a directory *and all its contents* if we use the
recursive option `-r`, and it will do so *without any confirmation prompts*:

~~~
$ rm -r thesis
~~~
{: .language-bash}

Given that there is no way to retrieve files deleted using the shell,
`rm -r` *should be used with great caution*
(you might consider adding the interactive option `rm -r -i`).

## Operations with multiple files and directories

Oftentimes one needs to copy or move several files at once.
This can be done by providing a list of individual filenames,
or specifying a naming pattern using wildcards.

> ## Copy with Multiple Filenames
>
> For this exercise, you can test the commands in the `shell-lesson-data/exercise-data` directory.
>
> In the example below, what does `cp` do when given several filenames and a directory name?
>
> ~~~
> $ mkdir backup
> $ cp creatures/minotaur.dat creatures/unicorn.dat backup/
> ~~~
> {: .language-bash}
>
> In the example below, what does `cp` do when given three or more file names?
>
> ~~~
> $ cd creatures
> $ ls -F
> ~~~
> {: .language-bash}
> ~~~
> basilisk.dat  minotaur.dat  unicorn.dat
> ~~~
> {: .output}
> ~~~
> $ cp minotaur.dat unicorn.dat basilisk.dat
> ~~~
> {: .language-bash}
>
> > ## Solution
> > If given more than one file name followed by a directory name
> > (i.e. the destination directory must be the last argument),
> > `cp` copies the files to the named directory.
> >
> > If given three file names, `cp` throws an error such as the one below,
> > because it is expecting a directory name as the last argument.
> >
> > ```
> > cp: target 'basilisk.dat' is not a directory
> > ```
> > {: .error}
> {: .solution}
{: .challenge}

### Using wildcards for accessing multiple files at once

> ## Wildcards
>
> `*` is a **wildcard**, which matches zero or more  characters.
> Let's consider the `shell-lesson-data/exercise-data/proteins` directory:
> `*.pdb` matches `ethane.pdb`, `propane.pdb`, and every
> file that ends with '.pdb'. On the other hand, `p*.pdb` only matches
> `pentane.pdb` and `propane.pdb`, because the 'p' at the front only
> matches filenames that begin with the letter 'p'.
>
> `?` is also a wildcard, but it matches exactly one character.
> So `?ethane.pdb` would match `methane.pdb` whereas
> `*ethane.pdb` matches both `ethane.pdb`, and `methane.pdb`.
>
> Wildcards can be used in combination with each other
> e.g. `???ane.pdb` matches three characters followed by `ane.pdb`,
> giving `cubane.pdb  ethane.pdb  octane.pdb`.
>
> When the shell sees a wildcard, it expands the wildcard to create a
> list of matching filenames *before* running the command that was
> asked for. As an exception, if a wildcard expression does not match
> any file, Bash will pass the expression as an argument to the command
> as it is. For example, typing `ls *.pdf` in the `proteins` directory
> (which contains only files with names ending with `.pdb`) results in
> an error message that there is no file called `*.pdf`.
> However, generally commands like `wc` and `ls` see the lists of
> file names matching these expressions, but not the wildcards
> themselves. It is the shell, not the other programs, that deals with
> expanding wildcards.
{: .callout}

> ## List filenames matching a pattern
>
> When run in the `proteins` directory, which `ls` command(s) will
> produce this output?
>
> `ethane.pdb   methane.pdb`
>
> 1. `ls *t*ane.pdb`
> 2. `ls *t?ne.*`
> 3. `ls *t??ne.pdb`
> 4. `ls ethane.*`
>
> > ## Solution
>>  The solution is `3.`
>>
>> `1.` shows all files whose names contain zero or more characters (`*`)
>> followed by the letter `t`,
>> then zero or more characters (`*`) followed by `ane.pdb`.
>> This gives `ethane.pdb  methane.pdb  octane.pdb  pentane.pdb`.
>>
>> `2.` shows all files whose names start with zero or more characters (`*`) followed by
>> the letter `t`,
>> then a single character (`?`), then `ne.` followed by zero or more characters (`*`).
>> This will give us `octane.pdb` and `pentane.pdb` but doesn't match anything
>> which ends in `thane.pdb`.
>>
>> `3.` fixes the problems of option 2 by matching two characters (`??`) between `t` and `ne`.
>> This is the solution.
>>
>> `4.` only shows files starting with `ethane.`.
> {: .solution}
{: .challenge}

> ## More on Wildcards
>
> Sam has a directory containing calibration data, datasets, and descriptions of
> the datasets:
>
> ~~~
> .
> ├── 2015-10-23-calibration.txt
> ├── 2015-10-23-dataset1.txt
> ├── 2015-10-23-dataset2.txt
> ├── 2015-10-23-dataset_overview.txt
> ├── 2015-10-26-calibration.txt
> ├── 2015-10-26-dataset1.txt
> ├── 2015-10-26-dataset2.txt
> ├── 2015-10-26-dataset_overview.txt
> ├── 2015-11-23-calibration.txt
> ├── 2015-11-23-dataset1.txt
> ├── 2015-11-23-dataset2.txt
> ├── 2015-11-23-dataset_overview.txt
> ├── backup
> │   ├── calibration
> │   └── datasets
> └── send_to_bob
>     ├── all_datasets_created_on_a_23rd
>     └── all_november_files
> ~~~
> {: .language-bash}
>
> Before heading off to another field trip, she wants to back up her data and
> send some datasets to her colleague Bob. Sam uses the following commands
> to get the job done:
>
> ~~~
> $ cp *dataset* backup/datasets
> $ cp ____calibration____ backup/calibration
> $ cp 2015-____-____ send_to_bob/all_november_files/
> $ cp ____ send_to_bob/all_datasets_created_on_a_23rd/
> ~~~
> {: .language-bash}
>
> Help Sam by filling in the blanks.
>
> The resulting directory structure should look like this
> ```
> .
> ├── 2015-10-23-calibration.txt
> ├── 2015-10-23-dataset1.txt
> ├── 2015-10-23-dataset2.txt
> ├── 2015-10-23-dataset_overview.txt
> ├── 2015-10-26-calibration.txt
> ├── 2015-10-26-dataset1.txt
> ├── 2015-10-26-dataset2.txt
> ├── 2015-10-26-dataset_overview.txt
> ├── 2015-11-23-calibration.txt
> ├── 2015-11-23-dataset1.txt
> ├── 2015-11-23-dataset2.txt
> ├── 2015-11-23-dataset_overview.txt
> ├── backup
> │   ├── calibration
> │   │   ├── 2015-10-23-calibration.txt
> │   │   ├── 2015-10-26-calibration.txt
> │   │   └── 2015-11-23-calibration.txt
> │   └── datasets
> │       ├── 2015-10-23-dataset1.txt
> │       ├── 2015-10-23-dataset2.txt
> │       ├── 2015-10-23-dataset_overview.txt
> │       ├── 2015-10-26-dataset1.txt
> │       ├── 2015-10-26-dataset2.txt
> │       ├── 2015-10-26-dataset_overview.txt
> │       ├── 2015-11-23-dataset1.txt
> │       ├── 2015-11-23-dataset2.txt
> │       └── 2015-11-23-dataset_overview.txt
> └── send_to_bob
>     ├── all_datasets_created_on_a_23rd
>     │   ├── 2015-10-23-dataset1.txt
>     │   ├── 2015-10-23-dataset2.txt
>     │   ├── 2015-10-23-dataset_overview.txt
>     │   ├── 2015-11-23-dataset1.txt
>     │   ├── 2015-11-23-dataset2.txt
>     │   └── 2015-11-23-dataset_overview.txt
>     └── all_november_files
>         ├── 2015-11-23-calibration.txt
>         ├── 2015-11-23-dataset1.txt
>         ├── 2015-11-23-dataset2.txt
>         └── 2015-11-23-dataset_overview.txt
> ```
> {: .language-bash}
>
> > ## Solution
> > ```
> > $ cp *calibration.txt backup/calibration
> > $ cp 2015-11-* send_to_bob/all_november_files/
> > $ cp *-23-dataset* send_to_bob/all_datasets_created_on_a_23rd/
> > ```
> > {: .language-bash}
> {: .solution}
{: .challenge}

> ## Organizing Directories and Files
>
> Jamie is working on a project and she sees that her files aren't very well
> organized:
>
> ~~~
> $ ls -F
> ~~~
> {: .language-bash}
> ~~~
> analyzed/  fructose.dat    raw/   sucrose.dat
> ~~~
> {: .output}
>
> The `fructose.dat` and `sucrose.dat` files contain output from her data
> analysis. What command(s) covered in this lesson does she need to run
> so that the commands below will produce the output shown?
>
> ~~~
> $ ls -F
> ~~~
> {: .language-bash}
> ~~~
> analyzed/   raw/
> ~~~
> {: .output}
> ~~~
> $ ls analyzed
> ~~~
> {: .language-bash}
> ~~~
> fructose.dat    sucrose.dat
> ~~~
> {: .output}
>
> > ## Solution
> > ```
> > mv *.dat analyzed
> > ```
> > {: .language-bash}
> > Jamie needs to move her files `fructose.dat` and `sucrose.dat` to the `analyzed` directory.
> > The shell will expand *.dat to match all .dat files in the current directory.
> > The `mv` command then moves the list of .dat files to the 'analyzed' directory.
> {: .solution}
{: .challenge}

> ## Reproduce a folder structure
>
> You're starting a new experiment and would like to duplicate the directory
> structure from your previous experiment so you can add new data.
>
> Assume that the previous experiment is in a folder called `2016-05-18`,
> which contains a `data` folder that in turn contains folders named `raw` and
> `processed` that contain data files.  The goal is to copy the folder structure
> of the `2016-05-18` folder into a folder called `2016-05-20`
> so that your final directory structure looks like this:
>
> ~~~
> 2016-05-20/
> └── data
>    ├── processed
>    └── raw
> ~~~
> {: .output}
>
> Which of the following set of commands would achieve this objective?
> What would the other commands do?
>
> ~~~
> $ mkdir 2016-05-20
> $ mkdir 2016-05-20/data
> $ mkdir 2016-05-20/data/processed
> $ mkdir 2016-05-20/data/raw
> ~~~
> {: .language-bash}
> ~~~
> $ mkdir 2016-05-20
> $ cd 2016-05-20
> $ mkdir data
> $ cd data
> $ mkdir raw processed
> ~~~
> {: .language-bash}
> ~~~
> $ mkdir 2016-05-20/data/raw
> $ mkdir 2016-05-20/data/processed
> ~~~
> {: .language-bash}
> ~~~
> $ mkdir -p 2016-05-20/data/raw
> $ mkdir -p 2016-05-20/data/processed
> ~~~
> {: .language-bash}
> ~~~
> $ mkdir 2016-05-20
> $ cd 2016-05-20
> $ mkdir data
> $ mkdir raw processed
> ~~~
> {: .language-bash}
> >
> > ## Solution
> > The first two sets of commands achieve this objective.
> > The first set uses relative paths to create the top-level directory before
> > the subdirectories.
> >
> > The third set of commands will give an error because the default behavior of `mkdir`
> > won't create a subdirectory of a non-existent directory:
> > the intermediate level folders must be created first.
> >
> > The fourth set of commands achieve this objective. Remember, the `-p` option,
> > followed by a path of one or more
> > directories, will cause `mkdir` to create any intermediate subdirectories as required.
> >
> > The final set of commands generates the 'raw' and 'processed' directories at the same level
> > as the 'data' directory.
> {: .solution}
{: .challenge}

## Combining multiple commands
Nothing prevents us from chaining pipes consecutively.
We can for example send the output of `wc` directly to `sort`,
and then the resulting output to `head`.
This removes the need for any intermediate files.

We'll start by using a pipe to send the output of `wc` to `sort`:

~~~
$ wc -l *.pdb | sort -n
~~~
{: .language-bash}

~~~
   9 methane.pdb
  12 ethane.pdb
  15 propane.pdb
  20 cubane.pdb
  21 pentane.pdb
  30 octane.pdb
 107 total
~~~
{: .output}

We can then send that output through another pipe, to `head`, so that the full pipeline becomes:

~~~
$ wc -l *.pdb | sort -n | head -n 1
~~~
{: .language-bash}

~~~
   9  methane.pdb
~~~
{: .output}

This is exactly like a mathematician nesting functions like *log(3x)*
and saying 'the log of three times *x*'.
In our case,
the calculation is 'head of sort of line count of `*.pdb`'.


The redirection and pipes used in the last few commands are illustrated below:

![Redirects and Pipes of different commands: "wc -l *.pdb" will direct the
output to the shell. "wc -l *.pdb > lengths" will direct output to the file
"lengths". "wc -l *.pdb | sort -n | head -n 1" will build a pipeline where the
output of the "wc" command is the input to the "sort" command, the output of
the "sort" command is the input to the "head" command and the output of the
"head" command is directed to the shell](../fig/redirects-and-pipes.svg)

> ## Piping Commands Together
>
> In our current directory, we want to find the 3 files which have the least number of
> lines. Which command listed below would work?
>
> 1. `wc -l * > sort -n > head -n 3`
> 2. `wc -l * | sort -n | head -n 1-3`
> 3. `wc -l * | head -n 3 | sort -n`
> 4. `wc -l * | sort -n | head -n 3`
>
> > ## Solution
> > Option 4 is the solution.
> > The pipe character `|` is used to connect the output from one command to
> > the input of another.
> > `>` is used to redirect standard output to a file.
> > Try it in the `shell-lesson-data/exercise-data/proteins` directory!
> {: .solution}
{: .challenge}


## Tools designed to work together
This idea of linking programs together is why Unix has been so successful.
Instead of creating enormous programs that try to do many different things,
Unix programmers focus on creating lots of simple tools that each do one job well,
and that work well with each other.
This programming model is called 'pipes and filters'.
We've already seen pipes;
a **filter** is a program like `wc` or `sort`
that transforms a stream of input into a stream of output.
Almost all of the standard Unix tools can work this way:
unless told to do otherwise,
they read from standard input,
do something with what they've read,
and write to standard output.

The key is that any program that reads lines of text from standard input
and writes lines of text to standard output
can be combined with every other program that behaves this way as well.
You can *and should* write your programs this way
so that you and other people can put those programs into pipes to multiply their power.


> ## Pipe Reading Comprehension
>
> A file called `animals.csv` (in the `shell-lesson-data/exercise-data/animal-counts` folder)
> contains the following data:
>
> ~~~
> 2012-11-05,deer,5
> 2012-11-05,rabbit,22
> 2012-11-05,raccoon,7
> 2012-11-06,rabbit,19
> 2012-11-06,deer,2
> 2012-11-06,fox,4
> 2012-11-07,rabbit,16
> 2012-11-07,bear,1
> ~~~
> {: .source}
>
> What text passes through each of the pipes and the final redirect in the pipeline below?
> Note, the `sort -r` command sorts in reverse order.
>
> ~~~
> $ cat animals.csv | head -n 5 | tail -n 3 | sort -r > final.txt
> ~~~
> {: .language-bash}
> Hint: build the pipeline up one command at a time to test your understanding
> > ## Solution
> > The `head` command extracts the first 5 lines from `animals.csv`.
> > Then, the last 3 lines are extracted from the previous 5 by using the `tail` command.
> > With the `sort -r` command those 3 lines are sorted in reverse order and finally,
> > the output is redirected to a file `final.txt`.
> > The content of this file can be checked by executing `cat final.txt`.
> > The file should contain the following lines:
> > ```
> > 2012-11-06,rabbit,19
> > 2012-11-06,deer,2
> > 2012-11-05,raccoon,7
> > ```
> > {: .source}
> {: .solution}
{: .challenge}

> ## Pipe Construction
>
> For the file `animals.csv` from the previous exercise, consider the following command:
>
> ~~~
> $ cut -d , -f 2 animals.csv
> ~~~
> {: .language-bash}
>
> The `cut` command is used to remove or 'cut out' certain sections of each line in the file,
> and `cut` expects the lines to be separated into columns by a <kbd>Tab</kbd> character.
> A character used in this way is a called a **delimiter**.
> In the example above we use the `-d` option to specify the comma as our delimiter character.
> We have also used the `-f` option to specify that we want to extract the second field (column).
> This gives the following output:
>
> ~~~
> deer
> rabbit
> raccoon
> rabbit
> deer
> fox
> rabbit
> bear
> ~~~
> {: .output}
>
> The `uniq` command filters out adjacent matching lines in a file.
> How could you extend this pipeline (using `uniq` and another command) to find
> out what animals the file contains (without any duplicates in their
> names)?
>
> > ## Solution
> > ```
> > $ cut -d , -f 2 animals.csv | sort | uniq
> > ```
> > {: .language-bash}
> {: .solution}
{: .challenge}

> ## Which Pipe?
>
> The file `animals.csv` contains 8 lines of data formatted as follows:
>
> ~~~
> 2012-11-05,deer,5
> 2012-11-05,rabbit,22
> 2012-11-05,raccoon,7
> 2012-11-06,rabbit,19
> ...
> ~~~
> {: .output}
>
> The `uniq` command has a `-c` option which gives a count of the
> number of times a line occurs in its input.  Assuming your current
> directory is `shell-lesson-data/exercise-data/animal-counts`,
> what command would you use to produce a table that shows
> the total count of each type of animal in the file?
>
> 1.  `sort animals.csv | uniq -c`
> 2.  `sort -t, -k2,2 animals.csv | uniq -c`
> 3.  `cut -d, -f 2 animals.csv | uniq -c`
> 4.  `cut -d, -f 2 animals.csv | sort | uniq -c`
> 5.  `cut -d, -f 2 animals.csv | sort | uniq -c | wc -l`
>
> > ## Solution
> > Option 4. is the correct answer.
> > If you have difficulty understanding why, try running the commands, or sub-sections of
> > the pipelines (make sure you are in the `shell-lesson-data/exercise-data/animal-counts`
> > directory).
> {: .solution}
{: .challenge}

## Executing Multiple Commands in one line
These concepts are important in bash.
> 1.  `A; B`    # Run command A and then B, regardless of success of A
> 2.  `A && B`  # Run command B if and only if A succeeded
> 3.  `A || B`  # Run command B if and only if A failed
> 4.  `A &`     # Run command A in background.

For example, we can type:
~~~
$ cd north-pacific-gyre
$ wc -l *.txt && echo "word count completed!"
~~~
{: .language-bash}


### Permissions in Unix

- "Correct permissions are critical for the security of a system."
- "File permissions describe who and what can read, write, modify, and access a file."
- "Use `ls -l` to view the permissions for a specific file."
- "Use `chmod` to change permissions on a file or directory."

Now let's look at files and directories.
Every file and directory on a Unix computer belongs to one owner and one group.
Along with each file's content,
the operating system stores the numeric IDs of the user and group that own it.

The user-and-group model means that
for each file
every user on the system falls into one of three categories:
the owner of the file,
someone in the file's group,
and everyone else.

For each of these three categories,
the computer keeps track of
whether people in that category can read the file,
write to the file,
or execute the file
(i.e., run it if it is a program).

For example, if a file had the following set of permissions:

<table class="table table-striped">
<tr><td></td><th>user</th><th>group</th><th>all</th></tr>
<tr><th>read</th><td>yes</td><td>yes</td><td>no</td></tr>
<tr><th>write</th><td>yes</td><td>no</td><td>no</td></tr>
<tr><th>execute</th><td>no</td><td>no</td><td>no</td></tr>
</table>

it would mean that:

*   the file's owner can read and write it, but not run it;
*   other people in the file's group can read it, but not modify it or run it; and
*   everybody else can do nothing with it at all.


{% include links.md %}

---
title: Setup
---
## Download files
You need to download some files to follow this lesson.

<!--
1. Download <a href="https://laitanawe.github.io/introscicomp/data/shell-lesson-data.zip">shell-lesson-data.zip</a> and move the file to your home directory.
2. Unzip/extract `shell-lesson-data.zip`
-->
1. Data for the class examples can be found at the following location:
/data/hps/assoc/private/intro_to_sci_comp/data

2. You can use the following commands to copy the data. If you want to highlight this command and copy, do not highlight the $:
    ~~~
    $ cp -Rv /data/hps/assoc/private/intro_to_sci_comp/data/shell-lesson-data /data/hps/assoc/private/intro_to_sci_comp/user/$USER
    ~~~
    {: .language-bash}

    ~~~
    $ cp -Rv /data/hps/assoc/private/intro_to_sci_comp/data/files /data/hps/assoc/private/intro_to_sci_comp/user/$USER
    ~~~
    {: .language-bash}

    ~~~
    $ ls data/hps/assoc/private/intro_to_sci_comp/user/$USER
    ~~~
    {: .language-bash}

    ~~~
    files  shell-lesson-data
    ~~~
    {: .output}

**Let your instructor know if you need help with this step**.
You should end up with the folder called **`shell-lesson-data`** in your user directory for the class.
You should also end up with certain files within the folder **`files`** in your user directory for the class.

### Associations on the Cluster
An Association is a managed shared workspace on Sasquatch that ensures reliable compute access, consistent software environments, and efficient collaboration and storage management. This is especially good for group projects and classes like Intro to Scientific Computing.

Why create association for this Linux class?
- Shared file system and resources, primarily:  When/if the cluster reaches high load, you may not have compute resources available during class time.  We'll need to use resource reservations to address this issue, and tying it with an Association is the easiest method to enable this.

<!--
For the Shell Scripts lesson, you need to download some files to follow the lesson.
1. Download <a href="https://laitanawe.github.io/introscicomp/data/bash-lesson.tar.gz">bash-lesson.tar.gz</a>
2. Make a directory called "files" on the Desktop
3. Move the bash-lesson.tar.gz inside the files directory and cd into the files directory.
4. Unzip/extract `bash-lesson.tar.gz` by typing `tar -xzvf bash-lesson.tar.gz`
You should end up certain files within the folder **`files`** in your home directory.

**Let your instructor know if you need help with this step**.
You should end up with a new folder called **`shell-lesson-data`** in your home directory.
-->

## If you need a terminal emulation software,
For a Windows computer, download and install <a href="https://mobaxterm.mobatek.net/download.html">MobaXterm</a>.

For a Mac computer running macOS, the default Unix Shell is Bash.

For a Linux computer, the default Unix Shell is usually Bash.

If you do not already have the shell software installed or do not use any of the operating
systems above, you may need to download and install it.

## Open a new shell
After installing the software
1. Open a terminal. If you're not sure how to open a terminal on your operating system,
see the instructions below.
2. In the terminal type `cd` then press the <kbd>Return</kbd> key.
   This step will make sure you start with your home folder as your working directory.

In the lesson, you will find out how to access the data files in this folder.

> ## Where to type commands: How to open a new shell
>
> The shell is a program that enables us to send commands to the computer and receive output.
> It is also referred to as the terminal or command line.
>
> Some computers include a default Unix Shell program.
> The steps below describe some methods for identifying and opening
> a Unix Shell program if you already have one installed.
> There are also options for identifying and downloading a Unix Shell program,
> a Linux/UNIX emulator, or a program to access a Unix Shell on a server.
>
> If none of the options below address your circumstances,
> try an online search for: Unix shell [your computer model] [your operating system].
{: .callout}

{::options parse_block_html="true" /}
<div>
<ul class="nav nav-tabs nav-justified" role="tablist">
<li role="presentation" class="active"><a data-os="windows" href="#windows" aria-controls="Windows"
role="tab" data-toggle="tab">Windows</a></li>
<li role="presentation"><a data-os="macos" href="#macos" aria-controls="macOS" role="tab"
data-toggle="tab">macOS</a></li>
<li role="presentation"><a data-os="linux" href="#linux" aria-controls="Linux" role="tab"
data-toggle="tab">Linux</a></li>
</ul>

<div class="tab-content">
<article role="tabpanel" class="tab-pane active" id="windows">
Computers with Windows operating systems do not automatically have a Unix Shell program
installed.
In this lesson, we encourage you to use an emulator included when you download and install <a href="https://mobaxterm.mobatek.net/download.html">MobaXterm</a> or [Git for Windows][install_shell],
which gives you access to both Bash shell commands and Git.

Once installed, you can open a terminal by running the program Git Bash from the Windows start
menu.

{% include links.md %}

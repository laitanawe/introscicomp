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

{% include links.md %}

---
title: "Installing Software"
teaching: 45
exercises: 45
questions:
- "How do you install software?"
- "How do we load and unload software packages?"
objectives:
- "Directly installing software"
- "Precompiled binaries"
- "Installing from the source"
- "Load and use a software package."
- "Explain how the shell environment changes when the module mechanism loads or unloads packages."
keypoints:
- "Load software with `module load softwareName`."
- "Unload software with `module unload`"
- "The module system handles software versioning and package conflicts for you
  automatically."
---
## Loading Software Modules

On a high-performance computing system, it is seldom the case that the software we want to use is available when we log in. It is installed, but we will need to "load" it before it can run.

Before we start using individual software packages, however, we should understand the reasoning behind this approach. The three biggest factors are:

- software incompatibilities
- versioning
- dependencies

Software incompatibility is a major headache for programmers. Sometimes the presence (or absence) of a software package will break others that depend on it. Two well known examples are Python and C compiler versions.
Python 3 famously provides a `python` command that conflicts with that provided by Python 2. Software compiled against a newer version of the C libraries and then run on a machine that has older C libraries installed will result in a nasty `'GLIBCXX_3.4.20' not found` error.

Software versioning is another common issue. A team might depend on a certain package version for their research project - if the software version was to change (for instance, if a package was updated), it might affect their results.
Having access to multiple software versions allows a set of researchers to prevent software versioning issues from affecting their results.

Dependencies are where a particular software package (or even a particular version) depends on having access to another software package (or even a particular version of another software package). For example, the VASP materials science software may depend on having a particular version of the FFTW (Fastest Fourier Transform in the West) software library available for it to work.

## Environment Modules

Environment modules are the solution to these problems. A _module_ is a self-contained description of a software package -- it contains the settings required to run a software package and, usually, encodes required dependencies on other software packages.

There are a number of different environment module implementations commonly used on HPC systems: the two most common are _TCL modules_ and _Lmod_. Both of these use similar syntax and the concepts are the same so learning to use one will allow you to use whichever is installed on the system you are using. In both implementations the `module` command is used to interact with environment modules. An additional subcommand is usually added to the command to specify what you want to do. For a list of subcommands you can use `module -h` or `module help`. As for all commands, you can access the full help on the _man_ pages with `man module`.

On login you may start out with a default set of modules loaded or you may start out with an empty environment.

### Listing Available Modules

To see available software modules, use `module avail`:

```
$ module avail
```
{: .language-bash}

```
-------------------------------------------------------------- /cm/local/modulefiles ---------------------------------------------------------------
boost/1.81.0             cm-bios-tools  dot              gpfs5/1.0.0      luajit        module-info  python3   slurm/slurm/24.11.3  
cluster-tools-dell/10.0  cmd            freeipmi/1.6.14  ipmitool/1.8.19  mariadb-libs  null         python39  
cluster-tools/10.0       cmjob          gcc/13.1.0       lua/5.4.6        module-git    openldap     shared    

-------------------------------------------------------------- /cm/shared/modulefiles --------------------------------------------------------------
blacs/openmpi/gcc/64/1.1patch03  gcc12/12.2.0                                   hpcx/mlnx-ofed5-cuda11/2.13.1/hpcx-stack  openmpi4/gcc/4.1.5  
blas/gcc/64/3.11.0               gdb/13.1                                       hwloc/1.11.13                             ucx/1.10.1          
bonnie++/2.00a                   globalarrays/openmpi/gcc/64/5.8                hwloc2/2.8.0                              
cm-pmix3/3.1.7                   gpfs5/1.0.0                                    intel-cluster-runtime/ia32/2019.6         
cm-pmix4/4.1.3                   hdf5/1.14.0                                    intel-cluster-runtime/intel64/(default)   
cuda11.8/blas/11.8.0             hdf5_18/1.8.21                                 intel-cluster-runtime/intel64/2019.6      
cuda11.8/fft/11.8.0              hpcx/2.4.0                                     iozone/3.494                              
cuda11.8/toolkit/11.8.0          hpcx/mlnx-ofed5-cuda11/2.13.1/hpcx             lapack/gcc/64/3.11.0                      
cuda12.2/blas/12.2.2             hpcx/mlnx-ofed5-cuda11/2.13.1/hpcx-debug       mpich/ge/gcc/64/4.1.1                     
cuda12.2/fft/12.2.2              hpcx/mlnx-ofed5-cuda11/2.13.1/hpcx-debug-ompi  mvapich2/gcc/64/2.3.7                     
cuda12.2/toolkit/12.2.2          hpcx/mlnx-ofed5-cuda11/2.13.1/hpcx-mt          netcdf/gcc/64/gcc/64/4.9.2                
cudnn8.6-cuda11.8/8.6.0.163      hpcx/mlnx-ofed5-cuda11/2.13.1/hpcx-mt-ompi     netperf/2.7.0                             
cudnn8.9-cuda12.2/8.9.7.29       hpcx/mlnx-ofed5-cuda11/2.13.1/hpcx-ompi        openblas/dynamic/(default)                
default-environment              hpcx/mlnx-ofed5-cuda11/2.13.1/hpcx-prof        openblas/dynamic/0.3.18                   
fftw3/openmpi/gcc/64/3.3.10      hpcx/mlnx-ofed5-cuda11/2.13.1/hpcx-prof-ompi   openmpi/gcc/64/4.1.5                      

-------------------------------------------------------------- /data/hps/assoc/module --------------------------------------------------------------
bioinformatics  

--------------------------------------------------- /data/hps/assoc/public/bioinformatics/module ---------------------------------------------------
0_modulefile_template           cellranger/7.2.0                   gatk/cliseqtools_apptainer          samtools/cliseqtools_apptainer      
azcopy/v10                      cellranger/8.0.0                   minimap2/cliseqtools_apptainer      STAR/2.7.11b                        
bcftools/cliseqtools_apptainer  cellranger/9.0.0                   multiqc/cliseqtools_apptainer       STAR_2.7.10a/cliseqtools_apptainer  
bedtools/cliseqtools_apptainer  cellranger/9.0.1                   picard-tools/cliseqtools_apptainer  trim_galore/cliseqtools_apptainer   
bowtie/cliseqtools_apptainer    cliSeqTools/cliseqtools_apptainer  R/R_apptainer                       
bwa/cliseqtools_apptainer       fastqc/cliseqtools_apptainer       salmon/cliseqtools_apptainer
```
{: .output}


### Listing Currently Loaded Modules

You can use the `module list` command to see which modules you currently have loaded in your environment. If you have no modules loaded, you will see a message telling you so

```
$ module list
```
{: .language-bash}

```
Currently Loaded Modulefiles:
 1) bioinformatics   2) gcc/13.1.0   3) slurm/slurm/24.11.3   4) python3 
```
{: .output}

## Loading and Unloading Software

To load a software module, use `module load`. In this example we will use Python 3.

Initially, Python 3 is not loaded. We can test this by using the `which` command. `which` looks for programs the same way that Bash does, so we can use it to tell us where a particular piece of software is stored.

```
$ which python3
```
{: .language-bash}

```
/usr/bin/python3
```
{: .output}

We can load the `python3` command with `module load python3`:

```
$ module load python3
$ which python3
```
{: .language-bash}

```
/cm/local/apps/python3/bin/python3
```
{: .output}


So, what just happened?

To understand the output, first we need to understand the nature of the `$PATH` environment variable. `$PATH` is a special environment variable that controls where a UNIX system looks for software. Specifically `$PATH` is a list of directories (separated by `:`) that the OS searches through for a command before giving up and telling us it can't find it. As with all environment
variables we can print it out using `echo`.

```
$ echo $PATH
```
{: .language-bash}

```
/cm/shared/apps/slurm/current/sbin:/cm/shared/apps/slurm/current/bin:/cm/local/apps/gcc/13.1.0/bin:/data/hps/home/oawe/.local/bin:/data/hps/home/oawe/bin:/cm/local/apps/environment-modules/4.5.3//bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/sbin:/usr/sbin:/cm/local/apps/environment-modules/4.5.3/bin:/opt/dell/srvadmin/bin
```
{: .output}

You'll notice a similarity to the output of the `which` command. In this case, there's only one difference: the different directory at the beginning. When we ran the `module load` command, it added a directory to the beginning of our `$PATH`. Let's examine what's there:

```
$ module unload python3
$ which python3
```
{: .language-bash}

```
/usr/bin/python3
```
{: .output}

Taking this to its conclusion, `module load` will add software to your `$PATH`. It "loads" software. A special note on this - depending on which version of the `module` program that is installed at your site, `module load` will also load required software dependencies.

<!--
```
```
{: .output}
-->

Note that this module loading process happens principally through the manipulation of environment variables like `$PATH`. There is usually little or no data transfer involved.

The module loading process manipulates other special environment variables as well, including variables that influence where the system looks for software libraries, and sometimes variables which tell commercial software packages where to find license servers.

The module command also restores these shell environment variables to their previous state when a module is unloaded.

## Directly Installing Software

The majority of Linux software can be installed by a regular user for their own use. No involvement from a system administrator is required. The easiest situation is when a binary executable is provided, but if you need to compile source code on Sasquatch that is also possible. Within your association directory there should be a bin directory, which is where executable software should go.

### Precompiled binaries

One example of software that is provided as a binary is SRA Toolkit. Here I am downloading it according to their instructions, into my group’s association. I unzip it and then change permissions so that the rest of my group can move or delete it if they need to.
```
$ cd /data/hps/assoc/private/intro_to_sci_comp/user/$USER/bin/
$ wget --output-document sratoolkit.tar.gz https://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/current/sratoolkit.current-centos_linux64.tar.gz
$ tar -xzvf sratoolkit.tar.gz
$ chmod -Rv 2775 sratoolkit.3.2.0-centos_linux64
$ ls -F
$ cd /data/hps/assoc/private/intro_to_sci_comp/user/$USER/bin/sratoolkit.3.2.0-centos_linux64/bin/
$ pwd
```
{: .language-bash}

Now copy the output from the pwd command and export it to your path. To add it to your path just for this session.
 
```
$ export PATH=$PATH:/data/hps/assoc/private/intro_to_sci_comp/user/$USER/bin/sratoolkit.3.2.0-centos_linux64/bin/
```
{: .language-bash}


It can be added just for this session when you run that export command from the command line, or it can be added by appending that export line line of code to ~/.bashrc if you always want to have SRA toolkit available. Remember the way to append things to a text file.
```
$ echo "export PATH=$PATH:/data/hps/assoc/private/intro_to_sci_comp/user/$USER/bin/sratoolkit.3.2.0-centos_linux64/bin/" >> ~/.bashrc
```
{: .language-bash}

You can check that fasterq-dump is on your path now:
```
$ which fasterq-dump
$ fasterq-dump --version
$ fasterq-dump --stdout -X 2 SRR390728
```
{: .language-bash}

### Software Installation from the Source

Each software package will have its own installation method, and therefore one has to carefully read the specific installation instructions before proceeding. However, a common pattern is to install a package from source code, using the following 3 steps:

- <b>configure</b>: Here, the Linux environment is checked for dependencies, and any user settings can be specified. This is the place where one would choose the final installation directory. As a regular user, this would be somewhere under the home directory, e.g. $HOME/path/to/software, or you can put it in the bin directory of your association.

```
$ cd /data/hps/assoc/private/intro_to_sci_comp/user/$USER/bin/sratoolkit.3.2.0-centos_linux64/bin/
$ pwd
```
{: .language-bash}

- <b>make</b>: Compiles the source code and produces the executables.

- <b>make install</b>: The executables are moved to their final destination and will be ready for use

Within your association directory, you should install new software into bin, whether downloading a standalone executable or compiling from the source. When compiling software from source code, you will most likely use the GNU Compiler Collection (gcc). There are multiple versions of the gcc installed on the HPC. The operating system and the system libraries are all compiled with one particular version. Unless you have a good reason to do otherwise, you should compile your custom software using the same version of gcc as the operating system. Please check your versions first and make sure they match.

Version of gcc used for system libraries:

```
$ strings -a /usr/lib/libc.so.6 | grep "GCC: ("
```
{: .language-bash}

Version of gcc used for compilation:
```
$ gcc -v 2>&1 | grep version
```
{: .language-bash}

### Downloading Software
We will install the program HMMER into a test directory as an example.

First, download the source code. Look up the website and find the link to the source code package appropriate for our system (Linux 64 bit). Then use program “curl” to retrieve the file archive:

```
$ cd /data/hps/assoc/private/intro_to_sci_comp/user/$USER/bin/sratoolkit.3.2.0-centos_linux64/bin/
$ curl -O http://eddylab.org/software/hmmer3/3.1b2/hmmer-3.1b2-linux-intel-x86_64.tar.gz
```
{: .language-bash}

Untar the archive, which will create directory hmmer-3.1b2-linux-intel-x86_64:

```
$ cd /data/hps/assoc/private/intro_to_sci_comp/user/$USER/bin/sratoolkit.3.2.0-centos_linux64/bin/
$ tar -xzvf hmmer-3.1b2-linux-intel-x86_64.tar.gz
```
{: .language-bash}

### Configure Software
Change directory to hmmer-3.1b2-linux-intel-x86_64. Look for an executable called “configure” and run it with a “help” argument, to view usage options. Look for an option that specifies the final destination of the software:
```
$ cd /data/hps/assoc/private/intro_to_sci_comp/user/$USER/bin/sratoolkit.3.2.0-centos_linux64/bin/
$ cd hmmer-3.1b2-linux-intel-x86_64
$ ./configure --help
```
{: .language-bash}

Notice in the output the following lines:
```
...
 
Installation directories:
  --prefix=PREFIX         install architecture-independent files in PREFIX
                          [/usr/local]
  --exec-prefix=EPREFIX   install architecture-dependent files in EPREFIX
                          [PREFIX]
 
By default, `make install' will install all the files in
`/usr/local/bin', `/usr/local/lib' etc.  You can specify
an installation prefix other than `/usr/local' using `--prefix',
for instance `--prefix=$HOME'.
 
...
```
{: .output}
Choose an installation prefix that puts the software into a test directory, just as an example. In a real installation you would have some naming pattern of your software directories that you would use. Now run configure for real:

```
$ cd /data/hps/assoc/private/intro_to_sci_comp/user/$USER/bin/sratoolkit.3.2.0-centos_linux64/bin/
$ installdir={{ < var path.examplelab >}}/bin }}
$ ./configure --prefix=$installdir/hmmer
```
{: .language-bash}
After making sure there were no errors during configuration, run the remaining 2 steps:

### Install Software using Make
```
$ cd /data/hps/assoc/private/intro_to_sci_comp/user/$USER/bin/sratoolkit.3.2.0-centos_linux64/bin/
$ make
$ make install
```
{: .language-bash}


Check that files have been placed in the chosen destination directory:
```
$ cd /data/hps/assoc/private/intro_to_sci_comp/user/$USER/bin/sratoolkit.3.2.0-centos_linux64/bin/
$ ls -lF $installdir/hmmer/
```
{: .language-bash}

```
total 0
drwxr-xr-x 2 mmouse upg-mmouse 4096 Oct 31 18:28 bin/
drwxr-xr-x 2 mmouse upg-mmouse 4096 Oct 31 18:28 include/
drwxr-xr-x 2 mmouse upg-mmouse 4096 Oct 31 18:28 lib/
drwxr-xr-x 4 mmouse upg-mmouse 4096 Oct 31 18:28 share/

```
{: .output}
Installation of the software is now complete. You can test that the software can be run:
```
$ cd /data/hps/assoc/private/intro_to_sci_comp/user/$USER/bin/sratoolkit.3.2.0-centos_linux64/bin/
$ ~/test/hmmer/bin/nhmmer -h
```
{: .language-bash}

```
# nhmmer :: search a DNA model or alignment against a DNA database
# HMMER 3.1b2 (February 2015); http://hmmer.org/
# Copyright (C) 2015 Howard Hughes Medical Institute.
# Freely distributed under the GNU General Public License (GPLv3).
# - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Usage: nhmmer [options] <query hmmfile|alignfile> <target seqfile>
 
Basic options:
  -h : show brief help on version and usage
 
Options directing output:
  -o <f>             : direct output to file <f>, not stdout
  -A <f>             : save multiple alignment of all hits to file <f>
...

```
{: .output}

### Clean up
You can clean up by removing the downloaded archive and the untared installer files
```
$ cd /data/hps/assoc/private/intro_to_sci_comp/user/$USER/bin/sratoolkit.3.2.0-centos_linux64/bin/
$ rm hmmer-3.1b2-linux-intel-x86_64.tar.gz
$ rm -r hmmer-3.1b2-linux-intel-x86_64
```
{: .language-bash}

{% include links.md %}

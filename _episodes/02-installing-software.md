---
title: "Installing Software"
teaching: 45
exercises: 45
questions:
- "How do we load and unload software packages?"
- "How do you install software?"
objectives:
- "Load and use a software package."
- "Explain how the shell environment changes when the module mechanism loads or unloads packages."
- "Directly installing software"
- "Precompiled binaries"
- "Installing from the source"
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

To ensure that you are working from your association directory, run the following command:

```
$ cd /data/hps/assoc/private/intro_to_sci_comp/user/$USER/
```
{: .language-bash}

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

Taking this to its conclusion, `module load` will add software to your `$PATH`. It "loads" software. A special note on this - depending on which version of the `module` program that is installed on the HPC cluster, `module load` will also load required software dependencies.

Let us use a bioinformatics tool like Fastqc on the cluster. Before loading the bioinformatics module, you get `command not found` when you run fastqc:
```
$ fastqc
```
{: .language-bash}

```
-bash: fastqc: command not found
```
{: .output}

Now load the bioinformatics module:
```
$ module load bioinformatics
```
{: .language-bash}

```
$ module load fastqc
```
{: .language-bash}

```
Use the following command to access common command line tools from the Bioinformatics CLI tools container.      
Please notice that this command adds /data/hps/assoc to the container's internal bindpaths so you can access files within your home or association shares.

        apptainer shell --bind /data/hps/assoc /data/hps/assoc/public/bioinformatics/container/cliSeqTools/BioinformaticsCliTools.sif
```
{: .output}

```
$ apptainer shell --bind /data/hps/assoc /data/hps/assoc/public/bioinformatics/container/cliSeqTools/BioinformaticsCliTools.sif
```
{: .language-bash}

You can run fastqc or other bioinformatics tools while you're within the container:
```
$ fastqc --help
```
{: .language-bash}

```
    -a              Specifies a non-default file which contains the list of
    --adapters      adapter sequences which will be explicity searched against
                    the library. The file must contain sets of named adapters
                    in the form name[tab]sequence.  Lines prefixed with a hash
                    will be ignored.

    -l              Specifies a non-default file which contains a set of criteria
    --limits        which will be used to determine the warn/error limits for the
                    various modules.  This file can also be used to selectively
                    remove some modules from the output all together.  The format
                    needs to mirror the default limits.txt file found in the
                    Configuration folder.

   -k --kmers       Specifies the length of Kmer to look for in the Kmer content
                    module. Specified Kmer length must be between 2 and 10. Default
                    length is 7 if not specified.

   -q --quiet       Supress all progress messages on stdout and only report errors.

   -d --dir         Selects a directory to be used for temporary files written when
                    generating report images. Defaults to system temp directory if
                    not specified.

BUGS

    Any bugs in fastqc should be reported either to simon.andrews@babraham.ac.uk
    or in www.bioinformatics.babraham.ac.uk/bugzilla/


Apptainer>
```
{: .output}

To exit out of the container:
```
Apptainer> exit
```
{: .language-bash}


<!--
```
```
{: .output}
-->

Note that this module loading process happens principally through the manipulation of environment variables like `$PATH`. There is usually little or no data transfer involved.

The module loading process manipulates other special environment variables as well, including variables that influence where the system looks for software libraries, and sometimes variables which tell commercial software packages where to find license servers.

The module command also restores these shell environment variables to their previous state when a module is unloaded.

## Containers: Docker vs Singularity/Apptainer

Containers are used for packaging software because they allow for reproducibility of research and reusability of code. Docker and Apptainer are two container technologies that are generally used by researchers but Apptainer is widely supported for scientific computing.
Docker is often blocked on HPC clusters but Singularity/Apptainer isn't.

The real reason comes to down to root privileges, which are the highest level of access on a Linux/Unix system. Both Docker and Singularity are designed to run on these systems.

The Docker daemon (that powers Docker containers) inherently requires root privileges. This allows the Docker daemon to create, modify, and delete files anywhere on the host by mounting host directories into the container on your behalf. This makes Docker very helpful when running on your local machine or on a virtual machine on the cloud where root privileges are allowed.

On HPC's, which are shared environments, users generally do not have root privileges. Rather, each user typically has a home directory with read-write privileges, a temporary scratch directory, and may have access to project directories (e.g., accessible by all users of a lab). You may have read access to system-wide directories, but having write-access would allow someone to have the ability to make system-wide changes (that's bad).

At this point, it's probably becoming clear why HPC administrators block Docker - the inherent root privileges of the Docker daemon are a security concern on a shared environment. It would allow the Docker container to write anywhere, potentially compromising the system or other users' data.

Singularity (aka Apptainer) was designed for shared environment HPCs. It is 'HPC-friendly' because it maps your user ID (and therefore your privileges) to the container you're running. For this reason, you cannot access directories while running a process inside a container that you wouldn't be able to as a user. A direct consequence of Singularity's non-root, user-mapped design, is that apart from your home directory (which is automatically mounted inside your container), you must explicitly bind other directories that you'd like to access inside of the container (such as your scratch or a project directory). If you attempt to bind a directory outside of your permissions, it will fail.

Thankfully, you can directly run Docker images with Singularity. When you run 'singularity pull docker://ubuntu:22.04', Singularity downloads the relevant Docker image layers and metadata from DockerHub and converts it in Singularity Image Format (SIF). Singularity can then use this SIF file to build and run a Singularity container.

{% include figure.html max-width="45%" caption=""
   file="/fig/docker_logo.jpeg"
   alt="Comparing Docker and Apptainer" %}
   {% include figure.html max-width="45%" caption=""
      file="/fig/apptainer_logo.jpeg"
      alt="Comparing Docker and Apptainer" %}

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

- <b>configure</b>: In this step, the Linux environment is checked for dependencies, and any user settings can be specified. This is the place where one would choose the final installation directory. As a regular user, this would be somewhere under the home directory, e.g. `$HOME/path/to/software`, or you can put it in the bin directory of your association.

```
$ cd /data/hps/assoc/private/intro_to_sci_comp/user/$USER/bin/
$ pwd
```
{: .language-bash}

- <b>make</b>: This step compiles the source code and produces the executables.

- <b>make install</b>: Here, the executables are moved to their final destination and will be ready for use

Within your association directory, you should install new software into bin, whether downloading a standalone executable or compiling from the source. When compiling software from source code, you will most likely use the GNU Compiler Collection (gcc). There are multiple versions of the gcc installed on the HPC. The operating system and the system libraries are all compiled with one particular version. Unless you have a good reason to do otherwise, you should compile your custom software using the same version of gcc as the operating system. Please check your versions first and make sure they match.

Version of gcc used for system libraries:

```
$ strings -a /usr/lib/libc.so.6 | grep "GCC: ("
```
{: .language-bash}

Version of gcc used for compilation:
```
$ gcc -v 2>&1
```
{: .language-bash}

```
$ gcc -v 2>&1 | grep version
```
{: .language-bash}

> ## Version of gcc used
>
> Using knowledge gained from our previous class, how else can you determine the version of gcc used for compilation?
>
> > ## Solution
> > $ gcc -v 2>&1 | tail -n 1
> {: .solution}
{: .challenge}

### Downloading Software
We will install the program HMMER into a test directory as an example.

First, download the source code. Look up the website and find the link to the source code package appropriate for our system (Linux 64 bit). Then use program “curl” to retrieve the file archive:

```
$ cd /data/hps/assoc/private/intro_to_sci_comp/user/$USER/bin/
$ curl -O http://eddylab.org/software/hmmer3/3.1b2/hmmer-3.1b2-linux-intel-x86_64.tar.gz
```
{: .language-bash}

Untar the archive, which will create directory hmmer-3.1b2-linux-intel-x86_64:

```
$ tar -xzvf hmmer-3.1b2-linux-intel-x86_64.tar.gz
$ ls -F
```
{: .language-bash}

```
hmmer-3.1b2-linux-intel-x86_64/        sratoolkit.3.2.0-centos_linux64/
hmmer-3.1b2-linux-intel-x86_64.tar.gz  sratoolkit.tar.gz
```
{: .output}

### Configure Software
Change directory to hmmer-3.1b2-linux-intel-x86_64. Look for an executable called “configure” and run it with a “help” argument, to view usage options. Look for an option that specifies the final destination of the software:
```
$ cd /data/hps/assoc/private/intro_to_sci_comp/$USER/bin/
$ cd hmmer-3.1b2-linux-intel-x86_64
$ ls -F
```
{: .language-bash}

```
aclocal.m4    config.sub    COPYRIGHT       include/    lib/            Makefile.in  release-notes  testsuite/
binaries/     configure*    documentation/  INSTALL     libdivsufsort/  profmark/    share/         tutorial/
config.guess  configure.ac  easel/          install-sh  LICENSE         README       src/           Userguide.pdf
```
{: .output}

```
$ ./configure --help
```
{: .language-bash}

Notice in the output the following lines:
```
...

                        disable compiler optimizations that would produce
                          unportable binaries
  --disable-pic           compile PIC objects [default=enabled for shared
                          builds on supported platforms]
  --disable-largefile     omit support for large files

Optional Packages:
  --with-PACKAGE[=ARG]    use PACKAGE [ARG=yes]
  --without-PACKAGE       do not use PACKAGE (same as --with-PACKAGE=no)
  --with-xlc-arch=<arch>  specify architecture <arch> for xlc -qarch
  --with-gsl              use the GSL, GNU Scientific Library (default is no)
  --with-gcc-arch=<arch>  use architecture <arch> for gcc -march/-mtune,
                          instead of guessing

Some influential environment variables:
  CC          C compiler command
  CFLAGS      C compiler flags
  LDFLAGS     linker flags, e.g. -L<lib dir> if you have libraries in a
              nonstandard directory <lib dir>
  LIBS        libraries to pass to the linker, e.g. -l<library>
  CPPFLAGS    (Objective) C/C++ preprocessor flags, e.g. -I<include dir> if
              you have headers in a nonstandard directory <include dir>
  MPICC       MPI C compiler command
  CPP         C preprocessor
  PIC_FLAGS   compiler flags for PIC code

Use these variables to override the choices made by `configure' or to help
it to find libraries and programs with nonstandard names/locations.

Report bugs to <eddys@janelia.hhmi.org>.

...
```
{: .output}
Choose an installation prefix that puts the software into a test directory, just as an example. In a real installation you would have some naming pattern of your software directories that you would use. Now run configure for real:

<!--
$ installdir={{ < var path.examplelab >}}/bin }}
-->

```
$ installdir=/data/hps/assoc/private/intro_to_sci_comp/user/$USER/bin/hmmer-3.1b2-linux-intel-x86_64
$ ./configure --prefix=$installdir/hmmer
```
{: .language-bash}

```
checking for ntohl... yes
checking for htons... yes
checking for htonl... yes
checking for _LARGEFILE_SOURCE value needed for large files... no
checking for special C compiler options needed for large files... no
checking for _FILE_OFFSET_BITS value needed for large files... no
configure: creating ./config.status
config.status: creating documentation/Makefile
config.status: creating documentation/man/Makefile
config.status: creating src/Makefile
config.status: creating testsuite/Makefile
config.status: creating profmark/Makefile
config.status: creating src/impl_sse/Makefile
config.status: creating easel/miniapps/Makefile
config.status: creating easel/testsuite/Makefile
config.status: creating easel/Makefile
config.status: creating libdivsufsort/Makefile
config.status: creating Makefile
config.status: creating src/p7_config.h
config.status: creating easel/esl_config.h
config.status: creating libdivsufsort/divsufsort.h
config.status: linking src/impl_sse to src/impl


HMMER configuration:
     compiler:             gcc -O3 -fomit-frame-pointer -fstrict-aliasing -march=amdfam10 -msse2  -fPIC
     host:                 x86_64-unknown-linux-gnu
     linker:               
     libraries:              
     DP implementation:    sse
```
{: .output}

After making sure there were no errors during configuration, run the remaining 2 steps:

### Install Software using Make
```
$ cd /data/hps/assoc/private/intro_to_sci_comp/user/$USER/bin/hmmer-3.1b2-linux-intel-x86_64
$ make
```
{: .language-bash}

```
     GEN jackhmmer
     CC phmmer.o
     GEN phmmer
     CC nhmmer.o
     GEN nhmmer
     CC nhmmscan.o
     GEN nhmmscan
     CC hmmpgmd.o
     GEN hmmpgmd
     CC hmmc2.o
hmmc2.c: In function ‘main’:
hmmc2.c:364:7: warning: ‘strncat’ specified bound 4096 equals destination size [-Wstringop-overflow=]
  364 |       strncat(opts, s,    MAX_READ_LEN);
      |       ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     GEN hmmc2
     CC makehmmerdb.o
     GEN makehmmerdb
     CC hmmerfm-exactmatch.o
     GEN hmmerfm-exactmatch
make[1]: Leaving directory '/data/hps/assoc/private/intro_to_sci_comp/user/oawe/bin/hmmer-3.1b2-linux-intel-x86_64/src'
     SUBDIR profmark
make[1]: Entering directory '/data/hps/assoc/private/intro_to_sci_comp/user/oawe/bin/hmmer-3.1b2-linux-intel-x86_64/profmark'
     CC create-profmark.o
     GEN create-profmark
     CC rocplot.o
     GEN rocplot
make[1]: Leaving directory '/data/hps/assoc/private/intro_to_sci_comp/user/oawe/bin/hmmer-3.1b2-linux-intel-x86_64/profmark'
```
{: .output}


```
$ make install
```
{: .language-bash}

```
...
for file in hmmer.h cachedb.h p7_gbands.h p7_gmxb.h p7_gmxchk.h p7_hmmcache.h; do \
   /usr/bin/install -c -m 0644 ./$file /data/hps/assoc/private/intro_to_sci_comp/user/oawe/bin/hmmer-3.1b2-linux-intel-x86_64/hmmer/include/ ;\
done
/usr/bin/install -c -m 0644 p7_config.h /data/hps/assoc/private/intro_to_sci_comp/user/oawe/bin/hmmer-3.1b2-linux-intel-x86_64/hmmer/include/ ;\

make[1]: Leaving directory '/data/hps/assoc/private/intro_to_sci_comp/user/oawe/bin/hmmer-3.1b2-linux-intel-x86_64/src'
     SUBDIR documentation
make[1]: Entering directory '/data/hps/assoc/private/intro_to_sci_comp/user/oawe/bin/hmmer-3.1b2-linux-intel-x86_64/documentation'
     SUBDIR man
make[2]: Entering directory '/data/hps/assoc/private/intro_to_sci_comp/user/oawe/bin/hmmer-3.1b2-linux-intel-x86_64/documentation/man'
for file in hmmer hmmalign hmmbuild hmmconvert hmmemit hmmfetch hmmlogo hmmpgmd hmmpress hmmscan hmmsearch hmmsim hmmstat jackhmmer makehmmerdb phmmer nhmmer nhmmscan alimask; do \
   /usr/bin/install -c -m 0755 ./$file.man /data/hps/assoc/private/intro_to_sci_comp/user/oawe/bin/hmmer-3.1b2-linux-intel-x86_64/hmmer/share/man/man1/${file}.1 ;\
done
make[2]: Leaving directory '/data/hps/assoc/private/intro_to_sci_comp/user/oawe/bin/hmmer-3.1b2-linux-intel-x86_64/documentation/man'
make[1]: Leaving directory '/data/hps/assoc/private/intro_to_sci_comp/user/oawe/bin/hmmer-3.1b2-linux-intel-x86_64/documentation'
```
{: .output}

Check that files have been placed in the chosen destination directory:
```
$ cd /data/hps/assoc/private/intro_to_sci_comp/user/$USER/bin/hmmer-3.1b2-linux-intel-x86_64
$ ls -lF $installdir/hmmer/
```
{: .language-bash}

```
total 4
drwxr-xr-x 2 oawe res-intro_to_sci_comp-s12-editor 4096 Nov 10 19:28 bin/
drwxr-xr-x 2 oawe res-intro_to_sci_comp-s12-editor 4096 Nov 10 19:28 include/
drwxr-xr-x 2 oawe res-intro_to_sci_comp-s12-editor 4096 Nov 10 19:28 lib/
drwxr-sr-x 4 oawe res-intro_to_sci_comp-s12-editor 4096 Nov 10 19:28 share/

```
{: .output}
Installation of the software is now complete. You can test that the software can be run:
```
$ cd /data/hps/assoc/private/intro_to_sci_comp/user/$USER/bin/hmmer-3.1b2-linux-intel-x86_64
$ /data/hps/assoc/private/intro_to_sci_comp/user/$USER/bin/hmmer-3.1b2-linux-intel-x86_64/hmmer/bin/nhmmer -h
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
  --tblout <f>       : save parseable table of hits to file <f>
  --dfamtblout <f>   : save table of hits to file, in Dfam format <f>
  --aliscoresout <f> : save scores for each position in each alignment to <f>
  --hmmout <f>       : if input is alignment(s), write produced hmms to file <f>
  --acc              : prefer accessions over names in output
  --noali            : don't output alignments, so output is smaller
  --notextw          : unlimit ASCII text output line width

...
  --F2 <x> : Stage 2 (Vit) threshold: promote hits w/ P <= F2  [3e-3]
  --F3 <x> : Stage 3 (Fwd) threshold: promote hits w/ P <= F3  [3e-5]
  --nobias : turn off composition bias filter

Options for selecting query alphabet rather than guessing it:
  --dna : input alignment is DNA sequence data
  --rna : input alignment is RNA sequence data

Options controlling seed search heuristic:
  --seed_max_depth <n>     : seed length at which bit threshold must be met  [15]
  --seed_sc_thresh <x>     : Default req. score for FM seed (bits)  [15]
  --seed_sc_density <x>    : seed must maintain this bit density from one of two ends  [0.8]
  --seed_drop_max_len <n>  : maximum run length with score under (max - [fm_drop_lim])  [4]
  --seed_drop_lim <x>      : in seed, max drop in a run of length [fm_drop_max_len]  [0.3]
  --seed_req_pos <n>       : minimum number consecutive positive scores in seed  [5]
  --seed_consens_match <n> : <n> consecutive matches to consensus will override score threshold  [11]
  --seed_ssv_length <n>    : length of window around FM seed to get full SSV diagonal  [70]

Other expert options:
  --tformat <s>      : assert target <seqdb> is in format <s>
  --qformat <s>      : assert query <seqfile> is in format <s>
  --nonull2          : turn off biased composition score corrections
  -Z <x>             : set database size (Megabases) to <x> for E-value calculations  (x>0)
  --seed <n>         : set RNG seed to <n> (if 0: one-time arbitrary seed)  [42]  (n>=0)
  --w_beta <x>       : tail mass at which window length is determined
  --w_length <n>     : window length - essentially max expected hit length
  --block_length <n> : length of blocks read from target database (threaded)   (n>=50000)
  --toponly          : only search the top strand
  --bottomonly       : only search the bottom strand
  --cpu <n>          : number of parallel CPU workers to use for multithreads  (n>=0)
...

```
{: .output}

### Clean up
You can clean up by removing the downloaded archive and the untared installer files
```
$ cd /data/hps/assoc/private/intro_to_sci_comp/user/$USER/bin/
$ rm -v hmmer-3.1b2-linux-intel-x86_64.tar.gz
```
{: .language-bash}

```
removed 'hmmer-3.1b2-linux-intel-x86_64.tar.gz'
```
{: .output}

Confirm that you're in your association directory before cleaning up or deleting files.
```
$ pwd
```
{: .language-bash}

```
/data/hps/assoc/private/intro_to_sci_comp/user/$USER/bin
```
{: .output}

```
$ rm -Rv hmmer-3.1b2-linux-intel-x86_64
```
{: .language-bash}

```
removed 'hmmer-3.1b2-linux-intel-x86_64/src/p7_alidisplay.o'
removed 'hmmer-3.1b2-linux-intel-x86_64/src/generic_decoding.c'
removed 'hmmer-3.1b2-linux-intel-x86_64/src/p7_bg.c'
removed 'hmmer-3.1b2-linux-intel-x86_64/src/p7_gmxb.o'
removed 'hmmer-3.1b2-linux-intel-x86_64/src/hmmlogo.c'
removed 'hmmer-3.1b2-linux-intel-x86_64/src/generic_fwdback_chk.o'
removed 'hmmer-3.1b2-linux-intel-x86_64/src/p7_hmmwindow.c'
removed 'hmmer-3.1b2-linux-intel-x86_64/src/generic_optacc.c'
removed 'hmmer-3.1b2-linux-intel-x86_64/src/hmmscan.c'
removed 'hmmer-3.1b2-linux-intel-x86_64/src/phmmer.o'
removed 'hmmer-3.1b2-linux-intel-x86_64/src/hmmpgmd.o'
removed 'hmmer-3.1b2-linux-intel-x86_64/src/phmmer.c'
removed 'hmmer-3.1b2-linux-intel-x86_64/src/p7_gmxchk.o'
removed 'hmmer-3.1b2-linux-intel-x86_64/src/generic_msv.o'
removed 'hmmer-3.1b2-linux-intel-x86_64/src/nhmmscan.c'
removed 'hmmer-3.1b2-linux-intel-x86_64/src/hmmsim.c'
removed 'hmmer-3.1b2-linux-intel-x86_64/src/p7_gmxchk.h'
removed 'hmmer-3.1b2-linux-intel-x86_64/src/logsum.c'
removed 'hmmer-3.1b2-linux-intel-x86_64/src/hmmer.o'
removed 'hmmer-3.1b2-linux-intel-x86_64/src/hmmalign.o'
removed 'hmmer-3.1b2-linux-intel-x86_64/src/evalues.o'
removed 'hmmer-3.1b2-linux-intel-x86_64/src/hmmdmstr.c'
removed 'hmmer-3.1b2-linux-intel-x86_64/src/hmmstat'
removed 'hmmer-3.1b2-linux-intel-x86_64/src/p7_null3.o'
removed 'hmmer-3.1b2-linux-intel-x86_64/src/hmmsim'
removed directory 'hmmer-3.1b2-linux-intel-x86_64/src'
removed directory 'hmmer-3.1b2-linux-intel-x86_64'
```
{: .output}

{% include links.md %}

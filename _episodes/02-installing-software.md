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



{% include links.md %}

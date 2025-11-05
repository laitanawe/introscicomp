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

{% include links.md %}

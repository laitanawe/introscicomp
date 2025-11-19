---
title: "SLURM Jobs"
teaching: 45
exercises: 45
questions:
- "What is a scheduler and why does a cluster need one?"
- How does my local computer compare to the remote systems?
- How does the login node compare to the compute nodes?
- Are all compute nodes alike?
- "How do I launch a program to run on a compute node in the cluster?"
- "How do I capture the output of a program that is run on a node in the cluster?"
- How can I be a responsible user of the HPC cluster?
- "How do we submit slurm jobs?"
objectives:
- Describe how the actions of a single user can affect the experience of others on a shared system.
- Discuss the behaviour of a considerate shared system citizen.
- JOB SUBMISSION
- "Batch Jobs"
- "Interactive Jobs"
- "SLURM Batch Scripts"
- "Mandatory Directives"
- "Resource Directives"
- "Recommended Service Units"
- JOB MANAGEMENT
- JOB ACCOUNTING
- "Tasks"
- "Submit a simple script to the cluster."
- "Monitor the execution of jobs using command line tools."
- "Inspect the output and error files of your jobs."
- "Find the right place to put large datasets on the cluster."
keypoints:
- An HPC system is a set of networked machines.
- HPC systems typically provide login nodes and a set of compute nodes.
- The resources found on independent (worker) nodes can vary in volume and type (amount of RAM, processor architecture, availability of network mounted filesystems, etc.).
- Files saved on shared storage are available on all nodes.
- The login node is a shared machine: be considerate of other users.
- "The queuing system facilitates executing tasks"
- "The scheduler handles how compute resources are shared between users."
- "A job is just a shell script."
- "Request _slightly_ more resources than you will need."
---
## Job Scheduler

High Performance Computing (HPC) systems are used for complex, data-intensive tasks like scientific simulations, artificial intelligence (AI) or machine learning model training, and large-scale data analysis across fields such as medicine, physics, finance, healthcare and life sciences. An HPC system might have thousands of nodes and thousands of users requesting resources. How do we decide who gets what and when? How do we ensure that a task is run with the resources it needs? This job is handled by a special piece of software called the _scheduler_. On an HPC system, the scheduler manages which jobs run where and when.

The following illustration compares these tasks of a job scheduler to a waiter in a restaurant. If you can relate to an instance where you had to wait for a while in a queue to get in to a popular restaurant, then you may now understand why sometimes your job do not start instantly as in your laptop.

{% include figure.html max-width="75%" caption=""
   file="/fig/restaurant_queue_manager.svg"
   alt="Compare a job scheduler to a waiter in a restaurant" %}
   
The scheduler used in this intro to scientific course is SLURM. Although SLURM is not used everywhere, running jobs is quite similar regardless of what software is being used. The exact syntax might change, but the concepts remain the same.

One of the major differences between using remote HPC resources and your own system (e.g. your laptop) is that remote resources are shared. How many users the resource is shared between at any one time varies from system to system, but it is unlikely you will ever be the only user logged into or using such a system.

The widespread usage of scheduling systems where users submit jobs on HPC resources is a natural outcome of the shared nature of these resources. There are other things you, as an upstanding member of the community, need to consider.

## Nodes

Individual computers that compose a cluster are typically called _nodes_
(although you will also hear people call them _servers_, _computers_ and
_machines_). On a cluster, there are different types of nodes for different
types of tasks. The node where you are right now is called the _login node_,
_head node_, _landing pad_, or _submit node_. A login node serves as an access
point to the cluster.

As a gateway, the login node should not be used for time-consuming or
resource-intensive tasks. You should be alert to this, and check with your
site's operators or documentation for details of what is and isn't allowed. It
is well suited for uploading and downloading files, setting up software, and
running tests. Generally speaking, in these lessons, we will avoid running jobs
on the login node.

Who else is logged in to the login node?

```
$ who
```
{: .language-bash}

```
lclar5   pts/2        2025-11-19 05:27 (10.90.98.58)
de-msemwa pts/6        2025-11-19 05:57 (10.89.46.211)
de-msemwa pts/63       2025-11-19 05:57 (10.89.46.211)
jmcdo3   pts/76       2025-10-29 14:30 (10.40.84.168)
de-ajosh3 pts/115      2025-11-19 07:00 (10.90.98.57)
root     pts/121      2025-11-06 14:48 (172:S.0)
dmache   pts/133      2025-10-20 15:29 (10.40.85.26)
jfra11   pts/152      2025-11-14 16:19 (10.40.84.108)
wli5     pts/140      2025-11-19 07:53 (10.90.98.62)
oawe     pts/169      2025-11-19 09:46 (172.28.128.7)
oawe     pts/172      2025-11-19 08:57 (172.28.128.7)
kchen7   pts/170      2025-11-19 08:51 (10.90.98.58)
jvale8   pts/153      2025-11-13 08:57 (10.45.87.229)
fduffy   pts/173      2025-11-19 09:13 (10.70.87.71)
lwei     pts/203      2025-11-17 14:01 (10.91.86.112)
```
{: .output}

Ideally, this should not show only your user ID, since there are likely several other people (including fellow learners in the class) connected right now.

The real work on a cluster gets done by the _compute_ (or _worker_) _nodes_.
compute nodes come in many shapes and sizes, but generally are dedicated to long
or hard tasks that require a lot of computational resources.

All interaction with the compute nodes is handled by a specialized piece of
software called a scheduler (the scheduler used in this lesson is called
SLURM. We'll learn more about how to use the
scheduler to submit jobs next, but for now, it can also tell us more
information about the compute nodes.

For example, we can view all of the compute nodes by running the command
`sinfo`.

```
[yourUsername@login1 ~]$ sinfo
```
{: .language-bash}

```
PARTITION                           AVAIL  TIMELIMIT  NODES  STATE NODELIST
cpu-core-sponsored                     up 14-00:00:0      1 drain* cpu-29
cpu-core-sponsored                     up 14-00:00:0      1   drng cpu-30
cpu-core-sponsored                     up 14-00:00:0      5    mix cpu-[1,6-9]
cpu-core-sponsored                     up 14-00:00:0      2  alloc cpu-[10-11]
cpu-core-sponsored                     up 14-00:00:0     21   idle cpu-[2-5,12-28]
cpu-core                               up 14-00:00:0      1 drain* cpu-29
cpu-core                               up 14-00:00:0      1   drng cpu-30
cpu-core                               up 14-00:00:0      5    mix cpu-[1,6-9]
cpu-core                               up 14-00:00:0      2  alloc cpu-[10-11]
cpu-core                               up 14-00:00:0     21   idle cpu-[2-5,12-28]
gpu-core-sponsored                     up 14-00:00:0      2   drng gpu-[1-2]
gpu-core-sponsored                     up 14-00:00:0      4   idle gpu-[3-6]
gpu-core                               up 14-00:00:0      2   drng gpu-[1-2]
gpu-core                               up 14-00:00:0      4   idle gpu-[3-6]
cpu-posit_workbench_small-sponsored    up   12:00:00      1    mix posit-1
cpu-posit_workbench_small-sponsored    up   12:00:00      2   idle cpu-[31-32]
cpu-posit_workbench-sponsored          up   12:00:00      1    mix posit-1
cpu-posit_workbench-sponsored          up   12:00:00      2   idle cpu-[31-32]
cpu-pwb_small                          up   12:00:00      1    mix posit-1
cpu-pwb_small                          up   12:00:00      2   idle cpu-[31-32]
cpu-pwb                                up   12:00:00      1    mix posit-1
cpu-pwb                                up   12:00:00      2   idle cpu-[31-32]
gpu-posit_workbench-sponsored          up   12:00:00      1   idle gpu-7
gpu-pwb                                up   12:00:00      1   idle gpu-7
cpu-test-sponsored                     up      15:00      1   idle cpu-33
cpu-test                               up      15:00      1   idle cpu-33
gpu-test-sponsored                     up      15:00      1   idle gpu-7
gpu-test                               up      15:00      1   idle gpu-7
```
{: .output}

A lot of the nodes are busy running work for other users: we are not alone
here!

There are also specialized machines used for managing disk storage, user
authentication, and other infrastructure-related tasks. Although we do not
typically logon to or interact with these machines directly, they enable a
number of key features like ensuring our user account and files are available
throughout the HPC system.

## Explore a Worker Node
Finally, let’s look at the resources available on the worker nodes where your jobs will actually run. `sinfo -o` specifies the `output_format` (%c is the Number of CPUs per node, %n to List of node hostnames,%m is the Size of memory per node in megabytes). We will pipe the output of the `sinfo` command into the column command. The column utility formats its input into multiple columns. Try running this command to see the name, CPUs and memory available on the worker nodes:
```
[yourUsername@login1 ~]$ sinfo -o "%n %c %m" | column -t
```
{: .language-bash}

## What's in a Node?

All of the nodes in an HPC system have the same components as your own laptop
or desktop: _CPUs_ (sometimes also called _processors_ or _cores_), _memory_
(or _RAM_), and _disk_ space. CPUs are a computer's tool for actually running
programs and calculations. Information about a current task is stored in the
computer's memory. Disk refers to all storage that can be accessed like a file
system. This is generally storage that can hold data permanently, i.e. data is
still there even if the computer has been restarted. While this storage can be
local (a hard drive installed inside of it), it is more common for nodes to
connect to a shared, remote fileserver or cluster of servers.

{% include figure.html url="" max-width="40%"
   file="/fig/node_anatomy.png"
   alt="Node anatomy" caption="" %}

> ## Explore the Login Node
>
> Now compare the resources of your computer with those of the login node.
>
> > ## Solution
> >
> > ```
> > [yourUsername@login1 ~]$ nproc --all
> > [yourUsername@login1 ~]$ free -m
> > ```
> > {: .language-bash}
> >
> > You can get more information about the processors using `lscpu`,
> > and a lot of detail about the memory by reading the file `/proc/meminfo`:
> >
> > ```
> > [yourUsername@login1 ~]$ less /proc/meminfo
> > ```
> > {: .language-bash}
> >
> > You can also explore the available filesystems using `df` to show **d**isk
> > **f**ree space. The `-h` flag renders the sizes in a human-friendly format,
> > i.e., GB instead of B. The **t**ype flag `-T` shows what kind of filesystem
> > each resource is.
> >
> > ```
> > [yourUsername@login1 ~]$ df -Th
> > ```
> > {: .language-bash}
> >
> > > ## Different results from `df`
> > >
> > > * The local filesystems (ext, tmp, xfs, zfs) will depend on whether
> > >   you're on the same login node (or compute node, later on).
> > > * Networked filesystems (beegfs, cifs, gpfs, nfs, pvfs) will be similar
> > >   -- but may include {{ site.remote.user }}, depending on how it
> > >   is [mounted][mount].
> > {: .discussion}
> >
> > > ## Shared Filesystems
> > >
> > > This is an important point to remember: files saved on one node
> > > (computer) are often available everywhere on the cluster!
> > {: .callout}
> {: .solution}
{: .challenge}



> ## Compare Your Computer, the Login Node and the Compute Node
>
> Compare your laptop's number of processors and memory with the numbers you
> see on the cluster login node and compute node. What implications do
> you think the differences might have on running your research work on the
> different systems and nodes?
>
> > ## Solution
> >
> > Compute nodes are usually built with processors that have _higher
> > core-counts_ than the login node or personal computers in order to support
> > highly parallel tasks. Compute nodes usually also have substantially _more
> > memory (RAM)_ installed than a personal computer. More cores tends to help
> > jobs that depend on some work that is easy to perform in _parallel_, and
> > more, faster memory is key for large or _complex numerical tasks_.
> {: .solution}
{: .discussion}

> ## Differences Between Nodes
>
> Many HPC clusters have a variety of nodes optimized for particular workloads.
> Some nodes may have larger amount of memory, or specialized resources such as
> Graphics Processing Units (GPUs or "video cards").
{: .callout}

With all of this in mind, we will now cover how to talk to the cluster's
scheduler, and use it to start running our scripts and programs!

## Be Kind to the Login Nodes

The login node is often busy managing all of the logged in users, creating and editing files and compiling software. If the machine runs out of memory or processing capacity, it will become very slow and unusable for everyone. While the machine is meant to be used, be sure to do so responsibly -- in ways that will not adversely impact other users' experience.

Login nodes are always the right place to launch jobs. Cluster policies vary, but they may also be used for proving out workflows, and in some cases, may host advanced cluster-specific debugging or development tools. The cluster may have modules that need to be loaded, possibly in a certain order, and paths or library versions that differ from your laptop, and doing an interactive test run on the head node is a quick and reliable way to discover and fix these issues.

> ## Login Nodes Are a Shared Resource
>
> Remember, the login node is shared with all other users and your actions
> could cause issues for other people. Think carefully about the potential
> implications of issuing commands that may use large amounts of resource.
>
> Unsure? Ask your friendly systems administrator ("sysadmin") if the thing
> you're contemplating is suitable for the login node, or if there's another
> mechanism to get it done safely.
{: .callout}

You can always use the commands `top` and `ps ux` to list the processes that are running on the login node along with the amount of CPU and memory they are using. If this check reveals that the login node is somewhat idle, you can safely use it for your non-routine processing task. If something goes wrong -- the process takes too long, or doesn't respond -- you can use the `kill` command along with the _PID_ to terminate the process.

> ## Login Node Etiquette
>
> Which of these commands would be a routine task to run on the login node?
>
> 1. `python physics_sim.py`
> 2. `make`
> 3. `create_directories.sh`
> 4. `molecular_dynamics_2`
> 5. `tar -xzf R-3.3.0.tar.gz`
>
> > ## Solution
> >
> > Building software, creating directories, and unpacking software are common
> > and acceptable > tasks for the login node: options #2 (`make`), #3
> > (`mkdir`), and #5 (`tar`) are probably OK. Note that script names do not
> > always reflect their contents: before launching #3, please
> > `less create_directories.sh` and make sure it's not a Trojan horse.
> >
> > Running resource-intensive applications is frowned upon. Unless you are
> > sure it will not affect other users, do not run jobs like #1 (`python`)
> > or #4 (custom MD code). If you're unsure, ask your friendly sysadmin for
> > advice.
> {: .solution}
{: .challenge}

If you experience performance issues with a login node you should report it to the system staff (usually via the helpdesk) for them to investigate.

Some SLURM commands can be seen on this page.

## Running a Batch Job

The most basic use of the scheduler is to run a command non-interactively. Any command (or series of commands) that you want to run on the cluster is called a _job_, and the process of using a scheduler to run the job is called _batch job
submission_.

In this case, the job we want to run is a shell script -- essentially a text file containing a list of UNIX commands to be executed in a sequential manner. Our shell script will have three parts:

* On the very first line, add `#!/usr/bin/env bash`. The `#!` (pronounced "hash-bang" or "shebang") tells the computer what program is meant to process the contents of this file. In this case, we are telling it
  that the commands that follow are written for the command-line shell (what we've been doing everything in so far).
* Anywhere below the first line, we'll add an `echo` command with a friendly greeting. When run, the shell script will print whatever comes after `echo` in the terminal.
  * `echo -n` will print everything that follows, _without_ ending the line by printing the new-line character.
* On the last line, we'll invoke the `hostname` command, which will print the name of the machine the script is run on.

```
[yourUsername@login1 ~]$ nano example-job.sh
```
{: .language-bash}
```
#!/usr/bin/env bash

echo -n "This script is running on "
hostname
```
{: .output}

> ## Creating Our Test Job
>
> Run the script. Does it execute on the cluster or just our login node?
>
> > ## Solution
> >
> > ```
> > [yourUsername@login1 ~]$ bash example-job.sh
> > ```
> > {: .language-bash}
> > ```
> > This script is running on login1
> > ```
> > {: .output}
> {: .solution}
{: .challenge}

This script ran on the login node, but we want to take advantage of the compute nodes: we need the scheduler to queue up `example-job.sh` to run on a compute node.

To submit this task to the scheduler, we use the `sbatch` command.
This creates a _job_ which will run the _script_ when _dispatched_ to a compute node which the queuing system has identified as being available to perform the work.

```
[yourUsername@login1 ~]$ sbatch example-job.sh
```
{: .language-bash}

```
Submitted batch job 7
```
{: .output}

And that's all we need to do to submit a job. Our work is done -- now the
scheduler takes over and tries to run the job for us. While the job is waiting
to run, it goes into a list of jobs called the _queue_. To check on our job's
status, we check the queue using the command
`squeue -u $USER`.

```
[yourUsername@login1 ~]$ squeue -u $USER
```
{: .language-bash}

```
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
    9 cpubase_b example-   user01  R       0:05      1 node1
```
{: .output}

We can see all the details of our job, most importantly that it is in the `R` or `RUNNING` state. Sometimes our jobs might need to wait in a queue (`PENDING`) or have an error (`E`).


> ## Where's the Output?
>
> On the login node, this script printed output to the terminal -- but
> now, when `squeue` shows the job has finished,
> nothing was printed to the terminal.
>
> Cluster job output is typically redirected to a file in the directory you
> launched it from. Use `ls` to find and `cat` to read the file.
{: .discussion}

## Customising a Job

The job we just ran used all of the scheduler's default options. In a
real-world scenario, that's probably not what we want. The default options
represent a reasonable minimum. Chances are, we will need more cores, more
memory, more time, among other special considerations. To get access to these
resources we must customize our job script.

Comments in UNIX shell scripts (denoted by `#`) are typically ignored, but
there are exceptions. For instance the special `#!` comment at the beginning of
scripts specifies what program should be used to run it (you'll typically see
`#!/usr/bin/env bash`). Schedulers like {{ site.sched.name }} also
have a special comment used to denote special scheduler-specific options.
Though these comments differ from scheduler to scheduler,
Slurm's special comment is `SBATCH`. Anything
following the `SBATCH` comment is interpreted as an
instruction to the scheduler.

Let's illustrate this by example. By default, a job's name is the name of the
script, but the `-J` option can be used to change the
name of a job. Add an option to the script:

```
[yourUsername@login1 ~]$ cat example-job.sh
```
{: .language-bash}

```
#!/usr/bin/env bash
SBATCH -J hello-world

echo -n "This script is running on "
hostname
```
{: .output}

Submit the job and monitor its status:

```
[yourUsername@login1 ~]$ sbatch example-job.sh
squeue -u $USER
```
{: .language-bash}

```
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
    9 cpubase_b example-   user01  R       0:05      1 node1
```
{: .output}

Fantastic, we've successfully changed the name of our job!

```
[yourUsername@login1 ~]$ nano example-fastq.sh
```
{: .language-bash}

```
#!/usr/bin/env bash
#SBATCH --job-name="my_job1"
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --time=0-00:05:00
#SBATCH --mem=1gb
#SBATCH --output="slurm-example-%j.o"
#SBATCH --error="slurm-example-%j.e"
#SBATCH --mail-user=youremail@seattlechildrens.org
#SBATCH --mail-type=BEGIN,END,FAIL,REQUEUE,ALL
#SBATCH --partition=cpu-test
#SBATCH --account=core

file=$1

cd /data/hps/assoc/private/intro_to_sci_comp/user/$USER/Desktop

echo "start"

echo the file being processed is $file
sleep 30

echo "end"
```
{: .output}

```
[yourUsername@login1 ~]$ sbatch example-fastq.sh
squeue -u $USER
```
{: .language-bash}

```
Submitted batch job 4625957
```
{: .output}

```
[yourUsername@login1 ~]$ sbatch example-fastq.sh
squeue -u $USER
```
{: .language-bash}

```
 JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
           4625958  cpu-test oawe_job     oawe  R       0:01      1 cpu-33
```
{: .output}

```
[yourUsername@login1 ~]$ ls -Ah
```
{: .language-bash}

```
example1.sbatch  orig_example1.sbatch     slurm-example-4625955.o  slurm-example-4625958.e
files            shell-lesson-data        slurm-example-4625957.e  slurm-example-4625958.o
jobsdir          slurm-example-4625955.e  slurm-example-4625957.o
```
{: .output}

```
[yourUsername@login1 ~]$ cat slurm-example-4625958.o
```
{: .language-bash}

```
start
the file being processed is
end
```
{: .output}

```
#!/usr/bin/env bash
#SBATCH --job-name="cellranger_job1"
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --time=0-01:00:00
#SBATCH --mem=1gb
#SBATCH --output="slurm-cellranger-%j.o"
#SBATCH --error="slurm-cellranger-%j.e"
#SBATCH --mail-user=youremail@seattlechildrens.org
#SBATCH --mail-type=BEGIN,END,FAIL,REQUEUE,ALL
#SBATCH --partition=cpu-core
#SBATCH --account=intro_to_sci_comp

echo "start" 
cd /data/hps/assoc/private/intro_to_sci_comp/user/$USER/Desktop
module load cellranger
cellranger testrun --id=tiny

echo the job is running
sleep 30

echo "end"
```
{: .output}

```
[yourUsername@login1 ~]$ sbatch cellranger1.slurm
```
{: .language-bash}


### Recommended syntax

Generally, all SLURM directives can be embedded into a Slurm script. However, it is better for certain directives to be retained on the command line and not incorporated.
- `--account` and `--partition` are parameters that can often change depending on who is running it and are dangerous to run blindly.
- Also `--mail-user` which specifies the email address will change depending on who the user is.
- Any directive specified on the command line will supersede those embedded in the script.
Edit the script and remove lines with the following directives from it.

- `#SBATCH --mail-user=youremail@seattlechildrens.org`
- `#SBATCH --partition=cpu-test`
- `#SBATCH --account=core`

Here is what is recommended:

```
sbatch --partition=<partition_name> --account=<account_name> --mail-user=<email> <batch_script.sh>
```
{: .language-bash}

```
sbatch --partition=cpu-core --account=intro_to_sci_comp --mail-user=yourusername@seattlechildrens.org example-job.sh
```
{: .language-bash}

The easiest way to list accounts and partitions available to you is through this command:
```
sshare -o "Account%40,Partition%40"
```
{: .language-bash}

### Resource Requests

What about more important changes, such as the number of cores and memory for
our jobs? One thing that is absolutely critical when working on an HPC system
is specifying the resources required to run a job. This allows the scheduler to
find the right time and place to schedule our job. If you do not specify
requirements (such as the amount of time you need), you will likely be stuck
with your site's default resources, which is proba
bly not what you want.

The following are several key resource requests:

`--ntasks=<ntasks>` or `-n <ntasks>`: How many CPU cores does your job need, in total?

`--time <days-hours:minutes:seconds>` or `-t <days-hours:minutes:seconds>`: How much real-world time (walltime) will your job take to run? The <days> part can be omitted.

`--mem=<megabytes>`: How much memory on a node does your job need in megabytes? You can also specify gigabytes using by adding a little “g” afterwards (example: `--mem=5g`)

`--nodes=<nnodes>` or `-N <nnodes>`: How many separate machines does your job need to run on? Note that if you set `ntasks` to a number greater than what one machine can offer, Slurm will set this value automatically.

Note that just _requesting_ these resources does not make your job run faster, nor does it necessarily mean that you will consume all of these resources. It only means that these are made available to you. Your job may end up using less memory, or less time, or fewer nodes than you have requested, and it will still run.

It's best if your requests accurately reflect your job's requirements. We'll talk more about how to make sure that you're using resources effectively in a later episode of this lesson.

> ## Submitting Resource Requests
>
> Modify our `hostname` script so that it runs for a minute, then submit a job for it on the cluster.
>
> 
>
> > ## Solution
> >
> > ```
> > [yourUsername@login1 ~]$ cat example-job.sh
> > ```
> > {: .language-bash}
> >
> > ```
> > #!/usr/bin/env bash
> > #SBATCH -t 00:01 # timeout in HH:MM
> >
> > echo -n "This script is running on "
> > sleep 20 # time in seconds
> > hostname
> > ```
> > {: .output}
> >
> > ```
> > [yourUsername@login1 ~]$ cat example-job.sh
> > ```
> > {: .language-bash}
> >
> > Why are the Slurm runtime and `sleep` time not identical?
> {: .solution}
{: .challenge}

Resource requests are typically binding. If you exceed them, your job will be
killed. Let's use wall time as an example. We will request 1 minute of
wall time, and attempt to run a job for four minutes.

```
[yourUsername@login1 ~]$ cat example-job.sh
```
{: .language-bash}

```
#!/usr/bin/env bash
#SBATCH -J long_job
#SBATCH -t 00:01 # timeout in HH:MM

echo "This script is running on ... "
sleep 240 # time in seconds
hostname
```
{: .output}

Submit the job and wait for it to finish. Once it is has finished, check the
log file.

```
[yourUsername@login1 ~]$ sbatch example-job.sh
[yourUsername@login1 ~]$ squeue -u $USER
```
{: .language-bash}

```
[yourUsername@login1 ~]$ cat slurm-12.out
```
{: .language-bash}

```
This script is running on ...
slurmstepd: error: *** JOB 12 ON node1 CANCELLED AT 2021-02-19T13:55:57
DUE TO TIME LIMIT ***
```
{: .output}

Our job was killed for exceeding the amount of resources it requested. Although
this appears harsh, this is actually a feature. Strict adherence to resource
requests allows the scheduler to find the best possible place for your jobs.
Even more importantly, it ensures that another user cannot use more resources
than they've been given. If another user messes up and accidentally attempts to
use all of the cores or memory on a node, {{ site.sched.name }} will either
restrain their job to the requested resources or kill the job outright. Other
jobs on the node will be unaffected. This means that one user cannot mess up
the experience of others, the only jobs affected by a mistake in scheduling
will be their own.

> ## Out of Time Jobs
>
> Run the script. Does it execute completely on the cluster or it just got killed?
>
> > ## Solution
> >
> > ```
> > [yourUsername@login1 ~]$ nano example-job.sh
> > ```
> > {: .language-bash}
> > ```
> > #!/usr/bin/env bash
> > #SBATCH --job-name="my_job1"
> > #SBATCH --nodes=1
> > #SBATCH --ntasks-per-node=1
> > #SBATCH --time=0-00:01:00
> > #SBATCH --mem=1gb
> > #SBATCH --output="slurm-example-%j.o"
> > #SBATCH --error="slurm-example-%j.e"
> > #SBATCH --mail-user=youremail@seattlechildrens.org
> > #SBATCH --mail-type=BEGIN,END,FAIL,REQUEUE,ALL
> > #SBATCH --partition=cpu-test
> > #SBATCH --account=core
> > 
> > file=$1
> > 
> > cd /data/hps/assoc/private/intro_to_sci_comp/user/$USER/Desktop
> > echo "start"
> > echo the file being processed is $file
> > sleep 30
> > echo "end"
> > ```
> > {: .output}
> {: .solution}
{: .challenge}

## Tasks
In the following example, we have TWO tasks, and then ask the computer to do 1 thing as a series of events.
```
#!/usr/bin/env bash
#SBATCH --job-name="my_tasks_job1"
#SBATCH --ntasks=1
#SBATCH --time=0-01:00:00
#SBATCH --mem=1gb
#SBATCH --output="slurm-tasks1-%j.o"
#SBATCH --error="slurm-tasks1-%j.e"
#SBATCH --mail-user=youremail@seattlechildrens.org
#SBATCH --mail-type=BEGIN,END,FAIL,REQUEUE,ALL
#SBATCH --partition=cpu-core
#SBATCH --account=intro_to_sci_comp

srun --ntasks=1 sleep 10 & 
srun --ntasks=1 sleep 12 &
wait
```
{: .language-bash}

In the following example, we have TWO tasks, and then ask the computer to do TWO things.
```
#!/usr/bin/env bash
#SBATCH --job-name="my_tasks_job2"
#SBATCH --ntasks=2
#SBATCH --time=0-01:00:00
#SBATCH --mem=1gb
#SBATCH --output="slurm-tasks2-%j.o"
#SBATCH --error="slurm-tasks2-%j.e"
#SBATCH --mail-user=youremail@seattlechildrens.org
#SBATCH --mail-type=BEGIN,END,FAIL,REQUEUE,ALL
#SBATCH --partition=cpu-core
#SBATCH --account=intro_to_sci_comp

srun --ntasks=1 sleep 10 & 
srun --ntasks=1 sleep 12 &
wait
```
{: .language-bash}

## Cancelling a Job

Sometimes we'll make a mistake and need to cancel a job. This can be done with the `scancel` command. Let's submit a job and then cancel it using its job number (remember to change the walltime so that it runs long enough for you to cancel it before it is killed!).

```
[yourUsername@login1 ~]$ sbatch example-job.sh
[yourUsername@login1 ~]$ squeue -u $USER
```
{: .language-bash}

```
Submitted batch job 13

JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
   13 cpubase_b long_job   user01  R       0:02      1 node1
```
{: .output}

Now cancel the job with its job number (printed in your terminal). A clean
return of your command prompt indicates that the request to cancel the job was
successful.

```
[yourUsername@login1 ~]$ scancel 38759
# It might take a minute for the job to disappear from the queue...
[yourUsername@login1 ~]$ squeue -u $USER
```
{: .language-bash}

```
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
```
{: .output}

> ## Cancelling multiple jobs
>
> We can also cancel all of our jobs at once using the `-u` option. This will delete all jobs for a specific user (in this case, yourself). Note that you can only delete your own jobs. Try submitting multiple jobs and then cancelling them all.
>
> 
>
> > ## Solution
> > First, submit a trio of jobs:
> > ```
> > [yourUsername@login1 ~]$ sbatch example-job.sh
> > [yourUsername@login1 ~]$ sbatch example-job.sh
> > [yourUsername@login1 ~]$ sbatch example-job.sh
> > ```
> > {: .language-bash}
> > 
> > Then, cancel them all:
> > ```
> > [yourUsername@login1 ~]$ scancel -u $USER
> > ```
> > {: .language-bash}
> >
> {: .solution}
{: .challenge}

## Other Types of Jobs

Up to this point, we've focused on running jobs in batch mode.
Slurm also provides the ability to start an interactive session.

There are very frequently tasks that need to be done interactively. Creating an
entire job script might be overkill, but the amount of resources required is
too much for a login node to handle. A good example of this might be building a
genome index for alignment with a tool like [HISAT2][hisat]. Fortunately, we
can run these types of tasks as a one-off with `srun`.

`srun` runs a single command on the cluster and then exits. Let’s demonstrate this by running the `hostname` command with `srun`. (We can cancel an srun job with `Ctrl-c`.)

```
[yourUsername@login1 ~]$ srun hostname
```
{: .language-bash}

```
node1
```
{: .output}

`srun` accepts all of the same options as `sbatch`. However, instead of specifying these in a script, these options are specified on the command-line when starting a job. To submit a job that uses 2 CPUs for instance, we could use the following command:
```
[yourUsername@login1 ~]$ srun -n 2 echo "This job will use 2 CPUs."
```
{: .language-bash}

```
This job will use 2 CPUs.
This job will use 2 CPUs.
```
{: .output}
Typically, the resulting shell environment will be the same as that for `sbatch`.

## Interactive Jobs
Sometimes, you will need a lot of resources for interactive use. Perhaps it’s our first time running an analysis or we are attempting to debug something that went wrong with a previous job. Fortunately, Slurm makes it easy to start an interactive job with `srun`:
```
[yourUsername@login1 ~]$ srun --pty bash
```
{: .language-bash}

You should be presented with a bash prompt. Note that the prompt will likely change to reflect your new location, in this case the compute node we are logged on. You can also verify this with `hostname`.

## Running a Job on a Compute Node

Create a submission file, requesting one task on a single node, then launch it.

```
nano serial-job.sh
cat serial-job.sh
```
{: .language-bash}

Use `ls` to locate the output file. The `-t` flag sorts in
reverse-chronological order: newest first. What was the output?

## Read the Job Output

The cluster output should be written to a file in the folder you launched the job from. For example,

```
ls -t
```
{: .language-bash}

<b>squeue</b> - View information about jobs.

<table class="table table-striped">
<tr><th>squeue option</th> <th>meaning</th><th></th><th></th></tr>
<tr><th>--account=account_name</th> <td>View only jobs with specified accounts.</td><td></td><td></td></tr>
<tr><th>--clusters=cluster_name</th> <td>View jobs on specified clusters.</td><td></td><td></td></tr>
<tr><th>--format=spec</th> <td>Output format to display. (e.g. "--format=¾i ¾j") Specify fields, size, order, etc.</td><td></td><td></td></tr>
<tr><th>--jobs=job_id_list</th> <td>Comma separated list of job IDs to display.</td><td></td><td></td></tr>
<tr><th>--name=job_name</th><td>View only jobs with specified names.</td><td></td><td></td></tr>
<tr><th>--partition=partition_names</th> <td>View only jobs in specified partitions.</td><td></td><td></td></tr>
<tr><th>--priority</th> <td>Sort jobs by priority.</td><td></td><td></td></tr>
<tr><th>--qos=qos_name</th> <td>View only jobs with specified Qualities Of Service.</td><td></td><td></td></tr>
<tr><th>--start</th> <td>Report the expected start time and resources to be allocated for pending jobs in order of increasing start time.</td><td></td><td></td></tr>
<tr><th>--state=state_names</th> <td>View only jobs with specified states.</td><td></td><td></td></tr>
<tr><th>--users=user_name</th> <td>View only jobs for specified users.</td><td></td><td></td></tr>
</table>

Job Submission
salloc - Obtain a job allocation.
sbatch - Submit a batch script for later execution.
srun - Obtain a job allocation (as needed) and execute an application.


## Software Versioning

We've learned how to load and unload software packages. This is very useful. However, we have not yet addressed the issue of software versioning. At some point or other, you will run into issues where only one particular version of some software will be suitable. Perhaps a key bugfix only happened in a certain version, or version X broke compatibility with a file format you use.
In either of these example cases, it helps to be very specific about what software is loaded.

Let's examine the usage of `module load` more closely. Save the following script as `fastqc.slurm`.

```
#!/usr/bin/env bash
#SBATCH --job-name="fastqc_job1"
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --time=0-01:00:00
#SBATCH --mem=1gb
#SBATCH --output="fastqc-slurm-example-%j.o"
#SBATCH --error="fastqc-slurm-example-%j.e"
#SBATCH --mail-user=youremail@seattlechildrens.org
#SBATCH --mail-type=BEGIN,END,FAIL,REQUEUE,ALL
#SBATCH --partition=cpu-core
#SBATCH --account=intro_to_sci_comp

file=$1

module load bioinformatics
module load fastqc

cd /data/hps/assoc/private/intro_to_sci_comp/user/$USER/Desktop

echo "start"

mkdir fqcreports

echo the file being processed is $file
apptainer exec --bind /data/hps/assoc /data/hps/assoc/public/bioinformatics/container/cliSeqTools/BioinformaticsCliTools.sif fastqc -o fqcreports $file
echo fastqc completed for $file
#sleep 130

echo "end"
```
{: .language-bash}

```
Submitted batch job 4671439
```
{: .output}

Save the following script as `cellranger1.slurm`
```
#!/usr/bin/env bash
#SBATCH --job-name="myjob_cellranger_job1"
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --time=0-01:00:00
#SBATCH --mem=1gb
#SBATCH --output="slurm-cellranger-%j.o"
#SBATCH --error="slurm-cellranger-%j.e"
#SBATCH --mail-user=youremail@seattlechildrens.org
#SBATCH --mail-type=BEGIN,END,FAIL,REQUEUE,ALL
#SBATCH --partition=cpu-core
#SBATCH --account=intro_to_sci_comp

begintime=$(date +'%d-%b-%Y %H:%M:%S')

module load cellranger

cellranger testrun --id=tiny

cd /data/hps/assoc/private/intro_to_sci_comp/user/$USER/Desktop

echo "start at "${begintime}

endtime=$(date +'%d-%b-%Y %H:%M:%S')

echo "end at "${endtime}
```
{: .language-bash}

```
sbatch cellranger1.slurm
```
{: .language-bash}

```
Submitted batch job 4671450
```
{: .output}

We've touched on all the skills you need to interact with an HPC cluster:
logging in over SSH, loading software modules, submitting jobs, and finding the output. Let's learn about estimating resource usage and why it might matter.

## Estimating Required Resources Using the Scheduler

Although we covered requesting resources from the scheduler earlier with the π code, how do we know what type of resources the software will need in the first place, and its demand for each? In general, unless the software documentation or user testimonials provide some idea, we won't know how much memory or compute time a program will need.

> ## Read the Documentation
>
> Most HPC facilities maintain documentation as a wiki, a website, or a
> document sent along when you register for an account. Take a look at these
> resources, and search for the software you plan to use: somebody might have
> written up guidance for getting the most out of it.
{: .callout}

A convenient way of figuring out the resources required for a job to run successfully is to submit a test job, and then ask the scheduler about its impact using `sacct -u $USER`. You can use this knowledge to set up the next job with a closer estimate of its load on the system. A good general rule is to ask the scheduler for 20% to 30% more time and memory than you expect the job to need. This ensures that minor fluctuations in run time or memory use will not result in your job being cancelled by the scheduler. Keep in mind that
if you ask for too much, your job may not run even though enough resources are available, because the scheduler will be waiting for other people's jobs to finish and free up the resources needed to match what you asked for.

## Stats

Since we already submitted our job to run on the cluster, we can query the scheduler to see how long our job took and what resources were used. We will use `sacct -u $USER` to get statistics about `job.sh`.

```
sacct -u $USER
```
{: .language-bash}

```
JobID           JobName  Partition    Account  AllocCPUS      State ExitCode 
------------ ---------- ---------- ---------- ---------- ---------- -------- 
4671314      oawe_fast+   cpu-core intro_to_+          1  COMPLETED      0:0 
4671314.bat+      batch            intro_to_+          1  COMPLETED      0:0 
4671314.ext+     extern            intro_to_+          1  COMPLETED      0:0 
4671316      oawe_fast+   cpu-core intro_to_+          1  COMPLETED      0:0 
4671316.bat+      batch            intro_to_+          1  COMPLETED      0:0 
4671316.ext+     extern            intro_to_+          1  COMPLETED      0:0 
4671317      oawe_fast+   cpu-core intro_to_+          1  COMPLETED      0:0 
4671317.bat+      batch            intro_to_+          1  COMPLETED      0:0 
4671317.ext+     extern            intro_to_+          1  COMPLETED      0:0 
4671318      oawe_fast+   cpu-core intro_to_+          1  COMPLETED      0:0 
4671318.bat+      batch            intro_to_+          1  COMPLETED      0:0 
4671318.ext+     extern            intro_to_+          1  COMPLETED      0:0 
4671319      oawe_fast+   cpu-core intro_to_+          1  COMPLETED      0:0 
4671319.bat+      batch            intro_to_+          1  COMPLETED      0:0 
4671319.ext+     extern            intro_to_+          1  COMPLETED      0:0 
4671320      oawe_fast+   cpu-core intro_to_+          1  COMPLETED      0:0 
4671320.bat+      batch            intro_to_+          1  COMPLETED      0:0 
4671320.ext+     extern            intro_to_+          1  COMPLETED      0:0 
4671321      oawe_fast+   cpu-core intro_to_+          1  COMPLETED      0:0 
4671321.bat+      batch            intro_to_+          1  COMPLETED      0:0 
4671321.ext+     extern            intro_to_+          1  COMPLETED      0:0 
4671322      oawe_fast+   cpu-core intro_to_+          1  COMPLETED      0:0 
4671322.bat+      batch            intro_to_+          1  COMPLETED      0:0 
4671322.ext+     extern            intro_to_+          1  COMPLETED      0:0 
4671323      oawe_fast+   cpu-core intro_to_+          1  COMPLETED      0:0 
4671323.bat+      batch            intro_to_+          1  COMPLETED      0:0 
4671323.ext+     extern            intro_to_+          1  COMPLETED      0:0 
4671324      oawe_fast+   cpu-core intro_to_+          1  COMPLETED      0:0 
4671324.bat+      batch            intro_to_+          1  COMPLETED      0:0 
4671324.ext+     extern            intro_to_+          1  COMPLETED      0:0 
4671325      oawe_fast+   cpu-core intro_to_+          1  COMPLETED      0:0 
4671325.bat+      batch            intro_to_+          1  COMPLETED      0:0 
4671325.ext+     extern            intro_to_+          1  COMPLETED      0:0 
4671326      oawe_fast+   cpu-core intro_to_+          1  COMPLETED      0:0 
4671326.bat+      batch            intro_to_+          1  COMPLETED      0:0 
4671326.ext+     extern            intro_to_+          1  COMPLETED      0:0 
4671327      oawe_fast+   cpu-core intro_to_+          1  COMPLETED      0:0 
4671327.bat+      batch            intro_to_+          1  COMPLETED      0:0 
4671327.ext+     extern            intro_to_+          1  COMPLETED      0:0 
4671328      oawe_fast+   cpu-core intro_to_+          1  COMPLETED      0:0 
4671328.bat+      batch            intro_to_+          1  COMPLETED      0:0 
4671328.ext+     extern            intro_to_+          1  COMPLETED      0:0 
4671329      oawe_fast+   cpu-core intro_to_+          1  COMPLETED      0:0 
4671329.bat+      batch            intro_to_+          1  COMPLETED      0:0 
4671329.ext+     extern            intro_to_+          1  COMPLETED      0:0 
4671330      oawe_fast+   cpu-core intro_to_+          1  COMPLETED      0:0 
4671330.bat+      batch            intro_to_+          1  COMPLETED      0:0 
4671330.ext+     extern            intro_to_+          1  COMPLETED      0:0 
4671331      oawe_cell+   cpu-core intro_to_+          1  COMPLETED      0:0 
4671331.bat+      batch            intro_to_+          1  COMPLETED      0:0 
4671331.ext+     extern            intro_to_+          1  COMPLETED      0:0
```
{: .output}

This shows all the jobs we ran today (note that there are multiple entries per job).
To get info about a specific job (for example, 347087), we change command slightly.
The entries ending in .bat+ and .ext+ in sacct output represent different job steps within a single Slurm job allocation. The plus sign (+) often indicates that the field name is truncated because the default display width is too narrow, but the full step names are batch and extern. 
bat+ (batch): This step accounts for the resources consumed by the main batch script itself. For many jobs, especially those that run a single, primary executable without using srun within the script, most of the resource usage (CPU time, memory, etc.) will be associated with this step.
ext+ (extern): This step accounts for any resource usage that occurs outside of Slurm's direct control but within the job's allocated cgroup/resource context on the compute node.
You can view the full names by adjusting the output format with the `--format` option, for example, sacct `--format=JobID%30,JobName%30` to widen the JobID and JobName fields.

```
[yourUsername@login1 ~]$ sacct -u $USER -l -j 4671331
```
{: .language-bash}

It will show a lot of info; in fact, every single piece of info collected on
your job by the scheduler will show up here. It may be useful to redirect this
information to `less` to make it easier to view (use the left and right arrow
keys to scroll through fields).

```
[yourUsername@login1 ~]$ sacct -u $USER -l -j 4671331 | less -S
```
{: .language-bash}

```
4671314
4671316
4671317
4671318
4671319
4671320
4671321
4671322
4671323
4671324
4671325
4671326
4671327
4671328
4671329
4671330
4671331
```
{: .language-bash}

```
[yourUsername@login1 ~]$ sacct -u $USER -l -j 4671331 | less -S
```
{: .output}

> ## Discussion
>
> This view can help compare the amount of time requested and actually
> used, duration of residence in the queue before launching, and memory
> footprint on the compute node(s).
>
> How accurate were our estimates?
{: .discussion}

## Improving Resource Requests

From the job history, we see that jobs finished executing in at most a few minutes, once dispatched. The time estimate we provided in the job script was far too long! This makes it harder for the queuing system to accurately estimate when resources will become free for other jobs. Practically, this means that the queuing system waits to dispatch our job until the full requested time slot opens, instead of "sneaking it in" a much shorter window where the job could actually finish. Specifying the expected runtime in the submission script more accurately will help alleviate cluster congestion and may get your job dispatched earlier.

> ## Narrow the Time Estimate
>
>  Edit `job.sh` to set a better time estimate. How close can
> you get?
>
>  Hint: use `-t`.
>
> > ## Solution
> >
> > The following line tells {{ site.sched.name }} that our job should
> > finish within 2 minutes:
> >
> > ```
> > #SBATCH -t 00:02:00
> > ```
> > {: .language-bash}
> {: .solution}
{: .challenge}

[hisat]: https://daehwankimlab.github.io/hisat2/
{% include links.md %}

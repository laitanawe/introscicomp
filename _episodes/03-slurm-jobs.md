---
title: "SLURM Jobs"
teaching: 45
exercises: 45
questions:
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
keypoints:
- "The queuing system facilitates executing tasks"
---
This lesson requires the terminal. Our shell will be bash.

One of the major differences between using remote HPC resources and your own system (e.g. your laptop) is that remote resources are shared. How many users the resource is shared between at any one time varies from system to system, but it is unlikely you will ever be the only user logged into or using such a system.

The widespread usage of scheduling systems where users submit jobs on HPC resources is a natural outcome of the shared nature of these resources. There are other things you, as an upstanding member of the community, need to consider.

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

Some SLURM commands can be seen on this page:

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

Let's examine the output of `module avail` more closely.

<table class="table table-striped">
<tr><th>slurm option</th> <th>meaning</th><th></th><th></th></tr>
<tr><th></th> <td></td> <td></td><td></td></tr>
<tr><th></th> <td>File in which to store job output.</td> <td></td><td></td></tr>
<tr><th></th> <td>Partition/queue in which to run the job.</td> <td></td><td></td></tr>
<tr><th></th> <td>Quality Of Service.</td> <td></td><td></td></tr>
<tr><th></th> <td>Signal job when approaching time limit.</td> <td></td><td></td></tr>
<tr><th></th> <td>Wall clock time limit.</td> <td></td><td></td></tr>
<tr><th></th> <td>Wrap specified command in a simple "sh" shell. (sbatch command only)</td> <td></td><td></td></tr>
</table>

<table class="table table-striped">
<tr><th>slurm option</th> <th>meaning</th><th></th><th></th></tr>
<tr><th></th> <td></td><td></td><td></td></tr>
<tr><th></th> <td></td><td></td><td></td></tr>
</table>

{% include links.md %}

---
title: "SLURM Jobs"
teaching: 45
exercises: 45
questions:
- "How do we submit slurm jobs?"
objectives:
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
This lesson requires a terminal application. Our shell will be bash.

Some SLURM commands can be found below:

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

<table class="table table-striped">
<tr><th>slurm option</th> <th>meaning</th><th></th><th></th></tr>
<tr><th></th> <td></td> <td></td><td></td></tr>
<tr><th></th> <td></td> <td></td><td></td></tr>
<tr><th></th> <td></td> <td></td><td></td></tr>
<tr><th></th> <td></td> <td></td><td></td></tr>
<tr><th></th> <td></td> <td></td><td></td></tr>
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

## Software Versioning

We've learned how to load and unload software packages. This is very useful. However, we have not yet addressed the issue of software versioning. At some point or other, you will run into issues where only one particular version of some software will be suitable. Perhaps a key bugfix only happened in a certain version, or version X broke compatibility with a file format you use.
In either of these example cases, it helps to be very specific about what software is loaded.

Let's examine the output of `module avail` more closely.

{% include links.md %}

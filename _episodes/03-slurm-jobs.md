---
title: "SLURM Jobs"
teaching: 45
exercises: 45
questions:
- "How do we submit slurm jobs?"
objectives:
- "Batch Jobs"
- "Interactive Jobs"
- "Mandatory Directives"
- "Resource Directives"
- "Recommended Service Units"
- "SLURM Batch Scripts"
- "Tasks"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---
This lesson requires a terminal application. Our shell will be bash.

Some SLURM commands can be found below:

<b>squeue</b> - View information about jobs.

<table class="table table-striped">
<tr><th>squeue option</th><th>meaning</th><th></th><th></th></tr>
<tr><td>--account=account_name</td><td>View only jobs with specified accounts.</td><th></th><th></th></tr>
<tr><th>read</th><td>yes</td><td>yes</td><td>no</td></tr>
<tr><th>write</th><td>yes</td><td>no</td><td>no</td></tr>
<tr><th>execute</th><td>no</td><td>no</td><td>no</td></tr>
</table>

<table class="table table-striped">
<tr><th>squeue option</th> <th>meaning</th><th></th><th></th></tr>
<tr><th></th> <td></td><td></td><td></td></tr>
<tr><th>--account=<name></th> <td>View only jobs with specified accounts.</td><td></td><td></td></tr>
<tr><th>--clusters=<name></th> <td>View jobs on specified clusters.</td><td></td><td></td></tr>
<tr><th>--format=<spec></th> <td>Output format to display. (e.g. "--format=¾i ¾j") Specify fields, size, order, etc.</td><td></td><td></td></tr>
<tr><th>--jobs=job_id_list</th> <td>Comma separated list of job IDs to display.</td><td></td><td></td></tr>
<tr><th>--name=<name></th><td>View only jobs with specified names.</td><td></td><td></td></tr>
<tr><th>--partition=<names></th> <td>View only jobs in specified partitions.</td><td></td><td></td></tr>
<tr><th>--priority</th> <td>Sort jobs by priority.</td><td></td><td></td></tr>
<tr><th>--qos=<name></th> <td>View only jobs with specified Qualities Of Service.</td><td></td><td></td></tr>
<tr><th>--start</th> <td>Report the expected start time and resources to be allocated for pending jobs in order of increasing start time.</td><td></td><td></td></tr>
<tr><th>--state=<names></th> <td>View only jobs with specified states.</td><td></td><td></td></tr>
<tr><th>--users=<names></th> <td>View only jobs for specified users.</td><td></td><td></td></tr>
</table>



{% include links.md %}

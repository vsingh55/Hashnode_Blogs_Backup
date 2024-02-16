---
title: "2.5 A Deep Dive into Task Scheduling with Crontab, Anacron, and At"
datePublished: Mon Jul 31 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cls2kzzwh000009lb3ftq5l42
slug: m2-operation-of-running-systems-1-1-1-1-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708108034822/6383c6c1-0003-4de2-bf4d-c5c1435f722f.png
tags: cloud, linux, devops, linux-for-beginners, devops-journey, 90daysofdevops, trainwithshubham

---

## Introduction

Task scheduling is a fundamental aspect of system administration, allowing users to automate routine processes on Unix-like systems. In this technical blog, we'll explore three powerful tools for scheduling tasks: Crontab, Anacron, and At. Each tool serves a unique purpose in orchestrating automated processes, providing users with flexibility and control over when tasks are executed.

### Crontab

Crontab, derived from "cron table," is a time-based job scheduler in Unix-like operating systems. It allows users to schedule recurring tasks at fixed intervals.

**Purpose**:

Crontab is designed for executing repetitive tasks automatically, such as system maintenance, backups, and periodic script executions.

**Syntax**

```bash
cron [-f] [-l] [-L loglevel]

-f : Used to stay in foreground mode, and don’t daemonize.
-l : This will enable the LSB compliant names for /etc/cron.d files.
-n : Used to add the FQDN in the subject when sending mails.
-L loglevel : This option will tell the cron what to log about the jobs with the following values: 
1 : It will log the start of all cron jobs.
2 : It will log the end of all cron jobs.
4 : It will log all the failed jobs. Here the exit status will not equal to zero.
8 : It will log the process number of all the cron jobs.
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706752824216/5f8adb91-f69a-4c88-83c4-74f629e99acc.png align="center")

**Examples**:

* Schedule a backup script to run every day at 3:00 AM:
    
    ```shell
    0 3 * * * /path/to/backup_script.sh
    ```
    
* Run a cleanup task every Sunday at midnight:
    
    ```shell
    0 0 * * 0 /path/to/cleanup_script.sh
    ```
    
* Execute a custom script every weekday at 8:30 AM:
    
    ```shell
    30 8 * * 1-5 /path/to/custom_script.sh
    ```
    
* Set up a monthly report generation task on the 15th at 6:00 PM:
    
    ```shell
    0 18 15 * * /path/to/report_generation.sh
    ```
    
* Run a task every 15 minutes:
    
    ```shell
    */15 * * * * /path/to/task.sh
    ```
    

### Anacron

Anacron is a task scheduler designed for systems that may not be running continuously. It ensures that scheduled tasks are executed, even if the system is powered off during the specified time.

**Purpose**:

Anacron is ideal for scenarios where tasks need to be performed regularly, but the system might not be online all the time, such as on laptops or intermittently used servers.

**Syntax**

```bash
anacron [-s]  [-f]  [-n] [-d] [-q] [-t anacrontab] [-S spooldir] [job]
anacron [-S spooldir] -u [-t anacrontab] [job] ...
anacron [-V|-h]
anacron -T [-t anacrontab]

f : Used to force execution of the jobs, ignoring the timestamps.
u : Only update the timestamps of the jobs, to the current date, but don’t run anything.
s : Serialize execution of jobs. Anacron will not start a new job before the previous one finished.
n : Run jobs now.Ignore any delay.
d : Don’t fork to the background. In this mode, Anacron will output informational messages to standard error, as well as to syslog. The output of jobs is mailed as usual.
q : Suppress messages to standard error. Only applicable with -d.
V (Use specified anacrontab) : Print version information and exit.
h (Use specified anacrontab) : Print short usage message, and exit.
```

**Examples**:

* Schedule a weekly system update check:
    
    ```shell
    @weekly /path/to/update_check.sh
    ```
    
* Run a monthly backup on the 1st day of each month:
    
    ```shell
    @monthly /path/to/backup_script.sh
    ```
    
* Execute a task every 12 hours:
    
    ```shell
    0 */12 * * * /path/to/task.sh
    ```
    
* Set up a task to run every day at 8:00 AM:
    
    ```shell
    @daily /path/to/daily_task.sh
    ```
    
* Ensure a task is executed every 10 days:
    
    ```shell
    0 0 */10 * * /path/to/ten_day_task.sh
    ```
    

**CRON vs ANACRON**

| `Cron` | `Anacron` |
| --- | --- |
| It’s a daemon | It’s not a daemon |
| Appropriate for server machines | Appropriate for desktop/laptop machines |
| Enables you to run scheduled jobs every minute | Only enables you to run scheduled jobs on daily basis |
| Doesn’t executed a scheduled job when the machine if off | If the machine if off when a scheduled job is due, it will execute a scheduled job when the machine is powered on the next time |
| Can be used by both normal users and root | Can only be used by root unless otherwise (enabled for normal users with specific configs) |

The major difference between **cron** and **anacron** is that **cron** works effectively on machines that will run continuously while **anacron** is intended for machines that will be powered off in a day or week.

### At

* `at` command is a command-line utility that is used to schedule a command to be executed at a particular time in the future.
    
* Jobs created with `at` command are executed **only once**.
    
* It executes commands at a particular time and accepts times of the form `HH:MM` to run a job at a specific time of day. The following expression like noon, midnight, teatime, tomorrow, next week, next Monday, etc. could be used with `at` command to schedule a job.
    

**Purpose**:

At is useful for scheduling one-time tasks or jobs that need to be executed at a specific moment in the future.

**Syntax:**

```bash
at [OPTION...] runtime
#press Ctrl+D to save job

at 15:00
warning: commands will be executed using /bin/sh
at> /usr/bin/touch file_created_by_at

#at a specific date
at 'August 20 2022'
#at a specific date and time
at '2:30 August 20 2022'

#using relative dates and time
# run 30 min later
at 'now + 30 minutes'

#run 3 hours later
at 'now + 3 hours'

#run 3 days later
at 'now + 3 days'

#run 3 weeks later
at 'now + 3 weeks'

#run 3 months later
at 'now + 3 months'

#to see which jobs are scheduled to run
atq
20 Wed Nov 17 08:30:00 2021 a aaron
# 20 is job ID

#if we forget what job is supposed to do use -c option and job ID
at -c 20
LESSOPEN=\|\|/usr/bin/lesspipe.sh\ %s; export LESSOPEN
cd /home/aaron || {
echo 'Execution directory inaccessible' >&2
exit 1
}
${SHELL:-/bin/sh} << 'marcinDELIMITER1d46213b'
command1
command2
marcinDELIMITER1d46213b

#to remove a command use atrm with job id
atrm 20
```

**Examples:**

Schedule a job to run at a specific time tomorrow:

* ```shell
      at 2:30 PM tomorrow
    ```
    
* Run a script in two hours:
    
    ```shell
    at now + 2 hours < /path/to/script.sh
    ```
    
* Schedule a task for midnight:
    
    ```shell
    at midnight
    ```
    
* Execute a command at a specific date and time:
    
    ```shell
    at 2024-02-01 15:45 < /path/to/task.sh
    ```
    
* Set up a job to run at 3:00 AM next Monday:
    
    ```shell
    at 3:00 AM next Monday
    ```
    

**Verify completion of scheduled jobs**

* Every `cron`, `anacron` and `at` jobs are logged in system.
    

```bash
sudo cat /var/log/cron
sudo grep CMD /var/log/cron
sudo grep anacron /var/log/cron
sudo grep atd /var/log/cron
```

### Conclusion

In conclusion, mastering the use of Crontab, Anacron, and At provides system administrators with powerful tools for automating tasks on Unix-like systems. Whether scheduling recurring jobs, accommodating intermittent system availability, or planning one-time executions, these tools offer flexibility and precision in managing routine processes.
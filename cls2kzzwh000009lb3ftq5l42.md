---
title: "M.2: Operation of Running Systems"
datePublished: Mon Jul 31 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cls2kzzwh000009lb3ftq5l42
slug: m2-operation-of-running-systems-1-1-1-1-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1706749481117/3af0f609-a3c7-4e87-b48b-acbfd425117d.png
tags: cloud, linux, devops, linux-for-beginners, 90daysofdevops

---

## Introduction

Task scheduling is a fundamental aspect of system administration, allowing users to automate routine processes on Unix-like systems. In this technical blog, we'll explore three powerful tools for scheduling tasks: Crontab, Anacron, and At. Each tool serves a unique purpose in orchestrating automated processes, providing users with flexibility and control over when tasks are executed.

### Crontab

Crontab, derived from "cron table," is a time-based job scheduler in Unix-like operating systems. It allows users to schedule recurring tasks at fixed intervals.

**Purpose**:

Crontab is designed for executing repetitive tasks automatically, such as system maintenance, backups, and periodic script executions.

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
    

### At

At is a command-line utility that schedules tasks to run only once at a specified time.

**Purpose**:

At is useful for scheduling one-time tasks or jobs that need to be executed at a specific moment in the future.

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
    

### Conclusion

In conclusion, mastering the use of Crontab, Anacron, and At provides system administrators with powerful tools for automating tasks on Unix-like systems. Whether scheduling recurring jobs, accommodating intermittent system availability, or planning one-time executions, these tools offer flexibility and precision in managing routine processes.
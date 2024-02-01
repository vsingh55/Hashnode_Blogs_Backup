---
title: "M.2: Operation of Running Systems"
datePublished: Wed Jul 26 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cls2k3g7g000009jlh1ohdboz
slug: m2-operation-of-running-systems-1-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1706749420188/25f2ee30-e61f-4c55-8791-1db59e7d9e01.png
tags: cloud, linux, devops, linux-for-beginners, 90daysofdevops

---

## Introduction:

Efficient process management is vital for maintaining system performance and stability on Linux systems. In this blog post, we'll explore three powerful commands - ps, pgrep, and nice - that facilitate the diagnosis and management of processes. Understanding these tools will empower system administrators and users to optimize system resources effectively.

### 1\. Overview of Processes:

Before delving into specific commands, let's briefly understand the basics of processes on Linux.

A process is a running instance of a program. Each process has a unique Process ID (PID) and consumes system resources. Managing processes involves tasks such as monitoring, terminating, and adjusting their priority.

### 2\. ps Command:

The ps command provides a snapshot of current processes on the system, offering detailed information about their status.

**Example 1: Displaying All Processes**

```bash
ps aux    # Display information about all processes
```

**Example 2: Sorting Processes by CPU Usage**

```bash
ps aux --sort=-%cpu    # Sort processes by CPU usage, highest first
```

### 3\. pgrep Command:

The pgrep command simplifies the process of finding and selecting processes based on criteria such as process names or attributes.

**Example 3: Finding PID of a Process by Name**

```bash
pgrep -o firefox    # Find the PID of the oldest Firefox process
```

**Example 4: Signaling Processes**

```bash
pkill -SIGTERM firefox    # Send the SIGTERM signal to terminate Firefox
```

### 4\. nice Command:

The nice command allows users to adjust the priority of processes, influencing their CPU scheduling.

**Example 5: Adjusting Process Priority**

```bash
nice -n 10 command    # Run 'command' with an increased priority
```

**Example 6: Running a Process with Lower Priority**

```bash
nice --20 command    # Run 'command' with a lower priority
```

### Conclusion:

Mastering the art of diagnosing and managing processes is essential for maintaining a healthy and responsive Linux system. The ps, pgrep, and nice commands offer a powerful trio of tools for monitoring, finding, and influencing the behavior of processes. Whether you are troubleshooting performance issues or optimizing resource usage, these commands provide the flexibility and control needed for effective process management on Linux.
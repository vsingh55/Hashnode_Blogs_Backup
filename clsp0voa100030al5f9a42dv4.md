---
title: "4.7 Keeping Time in Sync: Utilizing time_date_ctl for Network Time Synchronization"
datePublished: Fri Feb 16 2024 19:08:03 GMT+0000 (Coordinated Universal Time)
cuid: clsp0voa100030al5f9a42dv4
slug: 47-keeping-time-in-sync-utilizing-timedatectl-for-network-time-synchronization
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708110286454/6d7d068a-c575-4d9f-9d3e-4ff201a47afd.png
tags: linux, cloud-computing, devops, networking, linux-for-beginners, 90daysofdevops, shubhamlondhe, trainwithshubham

---

# **Introduction:**

Ensuring accurate timekeeping is crucial for various operations in a computer system, from logging events to coordinating tasks across multiple devices. One method to synchronize time across a network is by utilizing Network Time Protocol (NTP) servers. In this guide, we'll explore how to synchronize time using other network peers in Linux using the `timedatectl` command, particularly focusing on the `chronyd` implementation of NTP.

**Understanding Chrony and NTP:** Chrony is an implementation of the NTP protocol, designed to synchronize system clocks accurately with reference clocks or NTP servers. It offers flexibility in synchronization methods, including synchronization with NTP servers, reference clocks (e.g., GPS receivers), manual time inputs, and acting as an NTP server or peer.

**Using Chrony with** `timedatectl`: To synchronize time using other network peers with `chronyd`, we can leverage the `timedatectl` command along with `chronyc` for monitoring and management. Here's how to utilize these tools effectively:

1. **Checking Chrony Status:**
    
    ```bash
    systemctl status chronyd
    ```
    
    Use this command to check the status of the `chronyd` daemon, ensuring it's running and operational.
    
2. **Tracking Chrony Synchronization:**
    
    ```bash
    chronyc tracking
    ```
    
    This command provides detailed information about the system's synchronization status with NTP sources, including the offset, delay, and synchronization status.
    
3. **Viewing Chrony Sources:**
    
    ```bash
    chronyc sources
    ```
    
    Use this command to view information about the NTP sources configured in Chrony, including their status and reachability.
    
4. **Setting Timezone with** `timedatectl`:
    
    ```bash
    sudo timedatectl set-timezone America/New_York
    ```
    
    If `chronyd` is not installed, use `timedatectl` to set the timezone. Replace `America/New_York` with the desired timezone.
    
5. **Enabling NTP Synchronization:**
    
    ```bash
    sudo systemctl set-ntp true
    ```
    
    This command enables NTP synchronization, allowing the system clock to synchronize with NTP servers automatically.
    

```bash
#To check the status of chronyd use the following command:
systemctl status chronyd

#To Check Chrony Synchronization
chronyc tracking

#To check information about chrony’s sources:
chronyc sources

#Setting timezone if chronyd is not installed
#Ex set to Newyork
sudo timedatectl set-timezone America/New_York

#View all timezones available
timedatectl list-timezones

#If system clock is not in sync with NTP servers run
sudo systemctl set-ntp true
```

## **Conclusion:**

Synchronizing time using other network peers is essential for maintaining accurate timekeeping in Linux systems, especially in networked environments where coordination is critical. By leveraging tools like `chronyd` and `timedatectl`, administrators can ensure that system clocks remain synchronized with reliable NTP sources, minimizing discrepancies and improving system reliability. Understanding how to check synchronization status, configure timezones, and enable NTP synchronization is essential for effective time management in Linux environments.
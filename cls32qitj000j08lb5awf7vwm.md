---
title: "M.2: Operation of Running Systems"
datePublished: Sun Aug 06 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cls32qitj000j08lb5awf7vwm
slug: m2-operation-of-running-systems-1-1-1-1-1-1-1-1-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1706816267997/dfae2b26-600e-4737-9c8a-2e78c3dacd59.png
tags: cloud, linux, devops, linux-for-beginners, 90daysofdevops, shubhamlondhe, trainwithshubham

---

# Introduction:

Mandatory Access Controls (MAC) play a crucial role in enhancing the security of Linux systems. SELinux (Security-Enhanced Linux) and AppArmor are two popular MAC frameworks that enforce policies controlling the actions and permissions of processes. In this blog post, we will explore the semanage and getenforce commands, which are instrumental in listing and identifying SELinux and AppArmor file and process contexts.Understanding Security-Enhanced Linux (SELinux)

Security-Enhanced Linux, commonly known as SELinux, is a security architecture that is integrated into the Linux kernel. It uses the Linux Security Modules (LSM) to provide a robust and flexible Mandatory Access Control (MAC) system.

Under conventional Linux Discretionary Access Control (DAC), an application or a process that runs as a user (UID or SUID) possesses the user's permissions to various objects, such as files, sockets, and other processes. However, this DAC system can leave a system vulnerable to malicious or flawed applications that can potentially damage or destroy it.

Enter SELinux. By running a MAC kernel, SELinux provides an additional layer of security that protects the system against such threats.

So, how does SELinux work? It defines the access and transition rights of every user, application, process, and file on the system. It then governs the interactions of these entities using a comprehensive security policy. This policy specifies how strict or lenient a given Red Hat Enterprise Linux installation should be.

In essence, SELinux allows administrators to have more control over the system. It restricts the system to behave in only certain pre-defined ways, which significantly reduces the potential for exploitation.

However, with great power comes great responsibility. It is essential for system administrators to understand and configure SELinux correctly. Misconfigurations could lead to unnecessary restrictions or, worse, open up security holes.

In conclusion, SELinux is a powerful tool for managing system security. It offers a flexible and robust framework for controlling interactions between users and processes. By understanding and correctly implementing SELinux, system administrators can significantly enhance their system's security.

| **<mark>SELinux User</mark>** | **<mark>Roles</mark>** |
| --- | --- |
| **developer\_u** | developer\_r, docker\_r |
| **guest\_u** | guest\_r |
| **root** | staff\_r, sysadm\_r, system\_r, unconfined\_r |

## Understanding Access Control in Linux

When it comes to managing access control in Linux, the process is handled through a sophisticated system that involves various components. Here's a simplified explanation of how it works:

When a subject, such as an application, tries to access an object, like a file, the policy enforcement server located in the kernel checks an *Access Vector Cache* (AVC). This AVC is where the permissions for subjects and objects are cached.

If the AVC doesn't provide enough data for a decision to be made, the request then proceeds to the security server. This server will look up the *security context* of the application and file in a matrix. The security context is a set of security attributes associated with each subject and object.

Once the security server has this information, it will either grant or deny permission. If permission is denied, an `avc: denied` message is recorded in `/var/log/messages`. This provides a clear audit trail of access attempts and can be invaluable for troubleshooting or identifying potentially malicious activity.

The security context of subjects and objects is applied from the installed policy. This policy also provides the information needed to populate the security server's matrix. So, the policy plays a crucial role in this access control process.

Understanding this system is essential for system administrators and anyone responsible for managing access control in Linux. It provides a robust and flexible way of managing access rights, ensuring that subjects can only access the objects they're supposed to.

**The SELinux Decision Making Process**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706782754554/5db01dca-30f7-46e6-a1f4-ce2bde128a08.png align="center")

**SELinux Context Label**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706782824730/299d01b5-363a-438d-ac03-57adc933122c.png align="center")

* Every Linux user who logs into a system is mapped to an SELinux user as part of the SELinux policy configuration.
    
* Each user can only assume a predefined set of roles.
    
* If the role can be entered, next a check is performed to see \*\*\*\*if this role can transition to the type.
    
* The type or the name of this field comes from type enforcement and this type is like a protective software jail. Once something enters the type enforcement defined here, This restriction bubble is called type when dealing with files and domain when dealing with processes.
    
* Only a file marked with a certain type can start a process that can shift into a certain domain.
    
    SELinux basically go from user to role to type to enter into some kind of restricted secure box. Going through this three-step path, the security module achieves 3 important things.
    
    1. It makes sure that only certain users can enter certain roles and certain types. It keeps unauthorized users and processes from entering types and domains that are not for them.
        
    2. It lets authorized users and processes do their job by granting the permissions they need.
        
    3. At the same time,even authorized users and processes are allowed to take only a limited set of actions.
        
    4. Everything else is denied.
        
    
    Anything labeled with unconfined\_t is running unrestricted. SELinux lets those processes do almost anything they want.
    

```bash
#to see SELinux file and directory labels
ls -Z
unconfined_u:object_r:user_home_t:s0 archive.tar.gz

unconfined_u  :  object_r  :  user_home_t  :    s0
  user             role          type          level

#Anything labeled with unconfined_t is running unrestricted.
#SELinux lets those processes do almost anything they want.

#SELinux security context for processes
ps axZ
unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023 1875 ? Ss
 0:00 /usr/lib/
system_u:system_r:init_t:s0 1881 ? S 0:00 (sd-pam)
unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023 1891 ? Ssl
 0:00 /usr/bin

#To see the security context assigned to our current user
#(so what got applied to our user when we logged in)
id -Z
unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023

#When we log in, the user we log in as is automatically mapped 
#to an SELinux user, o see how this mapping is done
sudo semanage login -l
Login Name      SELinux User      MLS/MCS Range       Service
__default__     unconfined_u      s0-s0:c0.c1023         *
root            unconfined_u      s0-s0:c0.c1023         *

#To see the roles that each user can have on the system
sudo semanage user -l
SELinux User     Prefix     MCS Level    MCS Range
guest_u          user         s0           s0
root             user         s0       s0-s0:c0.c1023
staff_u          user         s0       s0-s0:c0.c1023
sysadm_u         user         s0       s0-s0:c0.c1023
```

```bash
#to see if SELinux status
#Enforcing : It's doing its job and denying authorized actions
#Permissive : it's allowing everything and just logging actions
#that should have been restricted instead of denying them
#Disabled means it's not doing anything, i
#t isn't enforcing security,and even not logging
getenforce
Enforcing

#or
sestatus
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             strict
Current mode:                   enforcing
Mode from config file:          enforcing
Policy MLS status:              disabled
Policy deny_unknown status:     denied
Max kernel policy version:      28

#change selinux status from enforcing to permissive
sudo setenforce 0
Permissive

#diable selinux permanently
sudo vi /etc/sysconfig/selinux
Change the SELINUX=enforcing directive to SELINUX=disabled

#Change the SELinux context or TYPE 
#(-t option to change the context of the file)
chcon -t httpd_sys_content_t index.html
```

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Stay tuned, Happy Learning | End of Module 2|</div>
</div>
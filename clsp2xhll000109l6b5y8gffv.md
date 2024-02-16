---
title: "5.12 Managing and Configuring Virtual Machines with QEMU, KVM, and VIRSH"
datePublished: Tue Sep 26 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clsp2xhll000109l6b5y8gffv
slug: 512-managing-and-configuring-virtual-machines-with-qemu-kvm-and-virsh
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708113685329/fac2cebc-02c7-459b-9d3a-e9f38667ac33.png
tags: linux, cloud-computing, linux-for-beginners, 90daysofdevops, shubhamlondhe, trainwithshubham

---

# **Introduction:**

QEMU (Quick Emulator) and KVM (Kernel-based Virtual Machine) are powerful tools for virtualization on Linux systems. Combined with the management utility VIRSH, they offer extensive capabilities for creating, managing, and configuring virtual machines (VMs). This guide outlines the steps to install these packages and demonstrates common commands for managing VMs.

**Installation:** To install QEMU, KVM, and the libvirt utilities, follow these steps:

```bash
sudo dnf install libvirt qemu-kvm
```

**Creating a Virtual Machine:** You can create a virtual machine by defining its configuration in an XML file. Here's an example of creating a basic virtual machine named "TestMachine":

```bash
virsh define testmachine.xml
```

**Managing Virtual Machines:** Once VMs are defined, you can manage them using VIRSH commands:

* List all active domains:
    
    ```bash
    virsh list
    ```
    
* List all domains (active and inactive):
    
    ```bash
    virsh list --all
    ```
    
* Start a virtual machine:
    
    ```bash
    virsh start testmachine
    ```
    
* Reboot a virtual machine:
    
    ```bash
    virsh reboot testmachine
    ```
    
* Force reset a virtual machine:
    
    ```bash
    virsh reset testmachine
    ```
    
* Shut down a virtual machine:
    
    ```bash
    virsh shutdown testmachine
    ```
    
* Forcefully power off a virtual machine:
    
    ```bash
    virsh destroy testmachine
    ```
    

**Deleting Virtual Machines:** To delete a virtual machine, first, shut it down and then undefine it:

* Undefine a VM (keeping its data):
    
    ```bash
    virsh undefine testmachine
    ```
    
* Undefine and remove all storage associated with the VM:
    
    ```bash
    virsh undefine --remove-all-storage testmachine
    ```
    

**Automatic Start on Boot:** You can configure a virtual machine to start automatically when the host machine boots:

* Enable auto-start:
    
    ```bash
    virsh autostart testmachine
    ```
    
* Disable auto-start:
    
    ```bash
    virsh autostart --disable testmachine
    ```
    

**Checking and Modifying Resources:** You can check and modify the resources allocated to a virtual machine:

* Check virtual machine information:
    
    ```bash
    virsh dominfo testmachine
    ```
    
* Modify resources (e.g., CPU):
    
    ```bash
    virsh setvcpus TestMachine 2 --config --maximum  # Set the maximum usable CPU to 2
    virsh setvcpus TestMachine 2 --config           # Set CPU to 2
    ```
    

## **Conclusion:**

With QEMU, KVM, and VIRSH, managing and configuring virtual machines on Linux systems becomes efficient and straightforward. These tools provide extensive capabilities for creating, starting, stopping, and modifying virtual machines, empowering users to build and manage virtualized environments effectively.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Congratulations! You've successfully completed Module 5 on service configuration. Well done on your learning journey so far!</div>
</div>
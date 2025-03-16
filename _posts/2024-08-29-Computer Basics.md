---
title: "Computer Basics"
date: 2024-08-29 00:00:00 +0800
categories: [Computer Basics]
tags: [Architecture]
---
These are different CPU architecture names, representing different types of processor designs:

1. **x86_64**:
    - Also known as **AMD64** or **Intel 64**.
    - This is a 64-bit version of the x86 architecture, widely used in desktops, laptops, and servers.
    - Compatible with most modern PCs.
2. **aarch64**:
	
    - Also known as **ARM64**.
    - This is the 64-bit extension of the ARM architecture, commonly used in mobile devices, tablets, and increasingly in servers and some desktops.
    - Known for energy efficiency.
3. ppc64le
4. :
    
    - Stands for **PowerPC 64-bit Little Endian**.
    - A 64-bit architecture used in IBM's PowerPC processors, often found in high-performance computing and enterprise systems.
    - "Little Endian" refers to the order in which bytes are stored in memory.
4. **s390x**:
    
    - A 64-bit architecture used in IBM's mainframe computers.
    - Known for reliability, availability, and serviceability (RAS), and commonly used in enterprise environments for large-scale computing tasks.

These architectures are significant because software must be compiled for the correct architecture to run on a given system.


The main differences between **cloud images**, **container images**, **Vagrant boxes**, and **Raspberry Pi** revolve around their intended use, platform, and deployment methods. Here's a breakdown of each:

### 1. **Cloud Images**
   - **Purpose**: Pre-configured disk images designed for deployment on cloud platforms such as AWS, Google Cloud, or Azure.
   - **Platform**: Cloud environments.
   - **Use Case**: Typically used to quickly spin up virtual machines (VMs) with pre-installed operating systems and sometimes applications.
   - **Deployment**: Launched directly on cloud platforms, often with cloud-init scripts for further customization.
   - **Examples**: Amazon Machine Images (AMI), Google Compute Engine images.

### 2. **Container Images**
   - **Purpose**: Lightweight, standalone, executable packages that include everything needed to run a piece of software, including code, runtime, libraries, and settings.
   - **Platform**: Container runtimes like Docker, Kubernetes.
   - **Use Case**: Running isolated applications or services in a consistent environment across different development, testing, and production environments.
   - **Deployment**: Deployed on container orchestrators like Docker, Kubernetes.
   - **Examples**: Docker images, OCI images.

### 3. **Vagrant Boxes**
   - **Purpose**: Virtual machine images designed for use with Vagrant, a tool for managing development environments.
   - **Platform**: Virtualization platforms like VirtualBox, VMware, or Hyper-V.
   - **Use Case**: Setting up consistent development environments that mirror production or staging environments.
   - **Deployment**: Managed by Vagrant, which automates the provisioning and configuration of the virtual machines.
   - **Examples**: Vagrant boxes for different operating systems and configurations (e.g., `ubuntu/xenial64`).

### 4. **Raspberry Pi**
   - **Purpose**: A small, single-board computer that runs various operating systems, primarily Linux distributions.
   - **Platform**: Raspberry Pi hardware.
   - **Use Case**: Hobbyist projects, IoT, embedded systems, and educational purposes.
   - **Deployment**: Uses custom or pre-built SD card images designed specifically for Raspberry Pi hardware.
   - **Examples**: Raspbian (now Raspberry Pi OS), Ubuntu for Raspberry Pi.


**Deployment Method***:
  - Cloud images are deployed via cloud providers.
  - Container images are deployed using container orchestration tools.
  - Vagrant boxes are deployed using Vagrant.
  - Raspberry Pi uses SD card images for booting.
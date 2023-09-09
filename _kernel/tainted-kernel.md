---
title: Tainted Kernel
---

# Tainted Kernel in Linux

In Linux, a "tainted" kernel refers to a kernel that is considered unreliable or potentially compromised due to the presence of non-GPL (General Public License) or proprietary kernel modules. The kernel can be marked as tainted for various reasons, including the use of third-party or closed-source kernel modules.

When the kernel is tainted, it means that certain functionality and support may be limited, and it may not be possible to provide accurate bug reports or receive support from the Linux community. The taint status is often indicated by a one-letter code in the kernel version information.

Common reasons for kernel tainting include:

1. **Proprietary Drivers**: Using proprietary graphics card or network drivers that are not open source.

2. **Loading Unsigned Modules**: Loading unsigned kernel modules that are not properly signed.

3. **Custom Kernel Modifications**: Making custom modifications to the kernel source code without adhering to open-source licensing.

4. **Out-of-Tree Modules**: Using out-of-tree modules that are not part of the mainline Linux kernel.

5. **Kernel Crashes**: Kernel crashes or panics caused by unsupported hardware or unstable modules.

It's important to note that a tainted kernel may lead to reduced stability and security risks. For this reason, it is generally recommended to use open-source and GPL-compliant kernel modules to maintain a clean (non-tainted) kernel.

To check if your kernel is tainted, you can use the `cat /proc/sys/kernel/tainted` command in a terminal. The value returned will indicate the taint status of your kernel.

In summary, a tainted kernel in Linux is one that is marked as unreliable or potentially compromised due to the use of non-GPL or proprietary kernel modules. Understanding kernel taint status is important for maintaining a stable and well-supported Linux system.


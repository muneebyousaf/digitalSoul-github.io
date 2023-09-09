---
title: Compiling and Loading an Out-of-Tree Kernel Module
---

# Compiling and Loading an Out-of-Tree Kernel Module

In Linux, kernel modules are pieces of code that can be dynamically loaded into the kernel to provide additional functionality or device support. Kernel modules have specific names associated with them, and it's important to avoid conflicts when compiling and loading them.

Sometimes, you may need to compile an out-of-tree kernel module with the same name as an existing module. This situation can arise when you are developing custom modules or using third-party modules that conflict with built-in kernel modules.

Here are the steps to compile and load an out-of-tree kernel module with the same name as an existing module:

**Note**: This example assumes you have a module named `my_module` and want to compile an out-of-tree module with the same name.

1. **Prepare Your Module Code**:
   - Create the source code for your out-of-tree module, ensuring it has a unique namespace to prevent conflicts.
   - Place the module code in a separate directory.

2. **Create a Makefile**:
   - Create a `Makefile` in the same directory as your module code.
   - In the `Makefile`, specify the module's name and source files.

   ```make
   obj-m += my_module.o

   all:
       make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules

   clean:
       make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
```

---
title:  Describing Non-Discoverable Hardware in Device Tree
---
# Describing Non-Discoverable Hardware in Device Tree

Device Tree is used to describe hardware components in embedded systems, including both discoverable and non-discoverable hardware. Non-discoverable hardware refers to devices that cannot be enumerated or identified by the operating system using standard methods such as PCI bus scanning.

In the Device Tree, non-discoverable hardware is typically described using custom nodes and properties to provide information about these components.

## Example: Describing a Non-Discoverable Hardware Component

Let's assume we have a custom hardware component that is not discoverable through standard mechanisms. We want to describe this component in the Device Tree.

1. Create a custom node for the hardware component:

```yaml
my_custom_hardware {
    compatible = "my,custom-hardware";
    reg = <0x10000000 0x1000>;  // Memory-mapped address and size
    interrupt-parent = <&intc>; // Reference to interrupt controller node
    interrupts = <0 0>;         // Interrupt properties
    // Add any other properties specific to your hardware
};
```

* **my_custom_hardware:** This is a custom node name for your hardware component.

* **compatible:** Specifies a string that identifies the hardware component. It can be used by the operating system to match the driver.

* **reg:** Specifies the memory-mapped address and size of the hardware component.

* **interrupt-parent  and interrupts:** Describe the interrupt properties of the hardware component.

* You can add any other custom properties relevant to your hardware.

# Relationship Between Non-Discoverable Hardware, Device Trees, and `fdt`/`fdt_` in U-Boot

In the context of embedded systems and bootloader environments like U-Boot, understanding the relationship between non-discoverable hardware, Device Trees (DT), and the `fdt` and `fdt_` APIs is crucial for configuring and managing hardware components during the boot process.

## Non-Discoverable Hardware

Non-discoverable hardware refers to hardware components that cannot be automatically detected or enumerated by the bootloader or operating system. This can include specialized peripherals, custom hardware interfaces, or legacy devices that lack Plug and Play (PnP) capabilities. In many cases, non-discoverable hardware requires manual configuration and initialization.

## Device Trees (DT)

Device Trees are hierarchical data structures used to describe the hardware components of a system in an abstracted and platform-independent manner. They provide a standardized way to represent hardware information, even for non-discoverable hardware. Device Trees are particularly valuable in embedded systems where hardware configurations can vary widely.

Key points regarding Device Trees:
- Device Trees describe the hardware topology, properties, and connections.
- Non-discoverable hardware components are often represented explicitly in the Device Tree, providing a clear definition of their existence and properties.
- Device Trees are typically loaded into memory during the bootloader phase (e.g., U-Boot) and later used by the Linux kernel for hardware initialization.

## `fdt` and `fdt_` APIs in U-Boot

U-Boot provides a set of `fdt` and `fdt_` APIs (Application Programming Interfaces) to interact with Device Trees. These APIs allow U-Boot to:
- Access information stored in the Device Tree.
- Modify the Device Tree to customize hardware configurations.
- Initialize non-discoverable hardware components.

The relationship between `fdt` and `fdt_` APIs and non-discoverable hardware is as follows:
- The `fdt` APIs enable U-Boot to read and manipulate Device Trees, making it possible to extract hardware information, configure properties, and even delete or add nodes and properties.
- U-Boot can use the `fdt_` APIs to perform specific operations on the Device Tree, such as retrieving property values, setting properties, and searching for strings or nodes within the Device Tree.
- When dealing with non-discoverable hardware, U-Boot may rely on the `fdt_` APIs to access and configure these components based on information provided in the Device Tree.

Overall, the `fdt` and `fdt_` APIs bridge the gap between non-discoverable hardware and software configuration in U-Boot, allowing bootloader developers to define and manage hardware characteristics, even for components that cannot be auto-detected.

Understanding this relationship is essential for booting embedded systems successfully, as it ensures that non-discoverable hardware is properly initialized and configured during the boot process.








# Explaining the `fdt` Command in U-Boot

In U-Boot, the `fdt` command is used to manipulate Device Trees (DT) and perform various operations related to Device Tree handling. Device Trees are data structures used to describe the hardware components of a system in a way that is abstracted from the underlying hardware architecture. The `fdt` command allows you to interact with these Device Trees in U-Boot.

## Common Subcommands and Operations

1. **fdt addr [addr]**: This command sets or displays the address of the loaded Device Tree blob in memory. It allows you to specify the memory location where the Device Tree is stored or view the current address.

   Example:
```
=> fdt addr 0x1000000 # Set the Device Tree address to 0x1000000
```


2. **fdt boardsetup**: This command calls the board-specific Device Tree setup function. It is often used to configure the hardware based on the information in the Device Tree.

Example:
```
=> fdt boardsetup

```


3. **fdt check [addr]**: This command checks the integrity and structure of the Device Tree at the specified address. It verifies whether the Device Tree blob is well-formed and valid.

Example:
```
=> fdt check 0x1000000

```





# Explaining `fdt_` APIs in U-Boot

In U-Boot, the `fdt_` APIs are a set of functions and macros used to work with Device Trees (DT) in various ways. These APIs allow you to manipulate and interact with Device Trees during the bootloader phase. Device Trees are data structures used to describe the hardware components of a system in an abstracted and platform-independent manner. The `fdt_` APIs provide a means to access and modify information within these Device Trees.

## Common `fdt_` APIs and Their Usage

1. **fdt_initrd(void *initrd_start, int initrd_size)**:
   - Initializes the initramfs (initial RAM filesystem) from the specified memory address and size.
   - Useful when booting with an initramfs image.

2. **fdtdec_get_int(const void *blob, int node, const char *propname, int default_val)**:
   - Retrieves an integer property value from the Device Tree blob.
   - Returns the default value if the property is not found.

3. **fdtdec_get_bool(const void *blob, int node, const char *propname, bool default_val)**:
   - Retrieves a boolean (true/false) property value from the Device Tree blob.
   - Returns the default value if the property is not found.

4. **fdt_get_addr(const void *blob, int node, const char *propname, phys_addr_t *addr)**:
   - Retrieves a physical address property value from the Device Tree blob.
   - Stores the address in the `addr` pointer.

5. **fdt_setprop(void *fdt, int nodeoffset, const char *name, const void *val, int len)**:
   - Sets a property with the specified name and value for a node in the Device Tree.
   - Allows you to add or modify properties in the Device Tree.

6. **fdt_del_node(void *fdt, int nodeoffset)**:
   - Deletes a node from the Device Tree, effectively removing a device or component.
   - Useful for device tree customization.

7. **fdt_next_node(const void *fdt, int nodeoffset)**:
   - Iterates through nodes in the Device Tree, returning the next node's offset.
   - Helps navigate the Device Tree structure.

8. **fdt_stringlist_search(const void *fdt, int nodeoffset, const char *property, const char *string)**:
   - Searches for a specific string within a property that contains a list of strings.
   - Useful for finding specific entries in lists.

These `fdt_` APIs provide a convenient way to access and manipulate Device Trees within U-Boot, enabling bootloader-level configuration and customization of hardware components and boot parameters.


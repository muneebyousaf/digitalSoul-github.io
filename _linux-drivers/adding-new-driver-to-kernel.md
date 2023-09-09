---
title: Adding a New Driver to Kernel Source
---

# Adding a New Driver to Kernel Source

To add a new driver to the Linux kernel sources, follow these steps:

1. **Add Your New Source File**: Place your new source file (e.g., `navman.c`) in the appropriate source directory within the Linux kernel source tree. For example, it would be `drivers/usb/serial/`.

2. **Describe the Configuration Interface**: Add the configuration interface for your new driver by editing the `Kconfig` file in the same directory where you added your source file. Include the following lines:

```make
config USB_SERIAL_NAVMAN
    tristate "USB Navman GPS device"
    depends on USB_SERIAL
    help
        To compile this driver as a module, choose M.
        If compiled as a module, it will be called navman.




- `config USB_SERIAL_NAVMAN`: Defines the configuration option for your driver. It gives your driver a unique identifier that can be used to enable or disable it during the kernel configuration process. The `config` keyword is used to declare a configuration option.

- `tristate "USB Navman GPS device"`: Sets the configuration option's name and description. In this case, the option is named "USB Navman GPS device." The `"tristate"` keyword indicates that the option can have three states: "Y" (yes, compile as part of the kernel), "M" (compile as a module), and "N" (do not compile). The description provides information about what the option represents.

- `depends on USB_SERIAL`: Specifies a dependency for your driver. It means that your driver depends on another configuration option called `USB_SERIAL` being enabled. If `USB_SERIAL` is not enabled, your driver will not be available for configuration or compilation.

- `help`: This keyword is followed by a brief description of your driver. It provides additional information about the purpose or functionality of your driver. Users configuring the kernel can refer to this help text to understand what the driver does.



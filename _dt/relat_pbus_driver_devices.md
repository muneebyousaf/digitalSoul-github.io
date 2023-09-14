---
title: Relationship between platform bus, drivers and devices 
---
# Relationship between platform bus, drivers and devices 
The relationship between the platform bus, platform drivers, and platform devices in the Linux kernel is fundamental to how the kernel manages and interacts with hardware components on embedded systems. These three components work together to provide a standardized and modular way to support and control hardware on a specific platform. Here's an overview of their relationship:

1. **Platform Bus:**
   - The platform bus is a framework or subsystem within the Linux kernel that serves as a bridge between platform-specific hardware components and device drivers.
   - It provides a standardized way for device drivers to discover and interact with hardware components on a particular platform.
   - The platform bus abstracts platform-specific details and hardware interfaces, making it easier to write device drivers that work across different hardware implementations of the same platform architecture.

2. **Platform Devices:**
   - Platform devices represent individual hardware components or peripherals integrated into the platform.
   - Each platform device is described in the platform-specific Device Tree (or board files) with information about its resources, such as memory-mapped I/O regions, interrupt lines, and other configuration details.
   - Platform devices are instantiated and registered with the kernel during platform initialization, typically at boot time.
   - They are identified by a unique name or ID, which is used to match them with the appropriate device drivers.

3. **Platform Drivers:**
   - Platform drivers are device drivers designed specifically to work with platform devices.
   - They abstract the hardware-specific details of the platform device and provide a standardized interface for higher-level software layers, such as the kernel and user-space applications.
   - Each platform driver is associated with one or more platform devices based on their unique names or IDs.
   - Platform drivers are implemented as kernel modules and follow the standard driver model in Linux.
   - They include callbacks for device initialization (probe), removal (remove), and handling hardware-specific operations.
   - The Linux kernel's driver framework matches platform drivers with platform devices based on their names or IDs and invokes the appropriate callbacks to initialize and manage the hardware.
## Example 
   
To illustrate the relationships between the platform bus, platform drivers, and platform devices in the Linux kernel, let's use a practical example of an embedded system based on the Raspberry Pi platform. We'll focus on GPIO (General-Purpose Input/Output) pins, which are commonly used for hardware control on embedded systems.

**Platform Devices and Device Tree:**

Suppose we have a Raspberry Pi-based embedded system, and we want to interact with a GPIO pin (e.g., GPIO pin 17) using a custom device driver. First, we describe this GPIO pin in the platform-specific Device Tree:

```dts
/ {
    compatible = "raspberrypi,3-model-b";
    
    gpio17: gpio@17 {
        compatible = "gpio-controller";
        reg = <0x3F200000>; // Physical memory address of GPIO controller
        gpio-controller;
        #gpio-cells = <2>;
    };
};
```

In this Device Tree snippet:

- `gpio17` is a platform device representing GPIO pin 17.
- It's described with its compatible string, resource information (memory-mapped I/O region), and GPIO controller properties.

**Platform Drivers:**

Now, let's say we want to create a custom device driver to control GPIO pin 17. We'll implement a platform driver for this purpose:
``` c 
#include <linux/init.h>
#include <linux/module.h>
#include <linux/platform_device.h>
#include <linux/gpio.h>

// GPIO pin number (17 on Raspberry Pi)
#define GPIO_PIN 17

static int my_gpio_probe(struct platform_device *pdev)
{
    int ret;

    // Request the GPIO pin
    ret = gpio_request(GPIO_PIN, "my-gpio-driver");
    if (ret) {
        pr_err("Failed to request GPIO %d\n", GPIO_PIN);
        return ret;
    }

    // Set the GPIO pin direction to output
    ret = gpio_direction_output(GPIO_PIN, 0); // Initialize to low
    if (ret) {
        pr_err("Failed to set GPIO direction\n");
        gpio_free(GPIO_PIN);
        return ret;
    }

    // Perform GPIO operations (e.g., set high)
    gpio_set_value(GPIO_PIN, 1); // Set to high

    pr_info("GPIO %d initialized and set to high\n", GPIO_PIN);
    return 0;
}

static int my_gpio_remove(struct platform_device *pdev)
{
    // Perform cleanup and release the GPIO pin
    gpio_set_value(GPIO_PIN, 0); // Set to low
    gpio_free(GPIO_PIN);

    pr_info("GPIO %d cleaned up and set to low\n", GPIO_PIN);
    return 0;
}

static struct platform_driver my_gpio_driver = {
    .probe = my_gpio_probe,
    .remove = my_gpio_remove,
    .driver = {
        .name = "my-gpio-driver",
        .owner = THIS_MODULE,
    },
};

module_platform_driver(my_gpio_driver);

MODULE_LICENSE("GPL");
MODULE_AUTHOR("Your Name");
MODULE_DESCRIPTION("Custom GPIO driver");
```

In this platform driver:

-   We've defined `GPIO_PIN` to specify the GPIO pin number (17 in this case).
    
-   In the `my_gpio_probe` function:
    
    -   We request the GPIO pin using `gpio_request`, providing a name for the driver.
    -   We set the GPIO pin direction to output using `gpio_direction_output`.
    -   We perform GPIO operations like setting the pin to high using `gpio_set_value`.
-   In the `my_gpio_remove` function:
    
    -   We perform cleanup operations, such as setting the GPIO pin to low.
    -   We release the GPIO pin using `gpio_free`.

These changes demonstrate how the platform driver interacts with the GPIO pin. When the driver is probed (loaded), it initializes the GPIO pin, and when it is removed (unloaded), it cleans up and releases the GPIO pin. The platform driver abstracts the hardware-specific details of GPIO pin control, providing a standardized interface for higher-level software layers to work with.

**Relationships:**

1. **Platform Device to Platform Driver Relationship:**
   - The Device Tree snippet describes `gpio17` as a platform device with compatible properties.
   - The platform driver, `my-gpio-driver`, has a `.name` field that matches the platform device's compatible string.
   - This matching is how the kernel associates the platform driver with the platform device.

2. **Platform Bus:**
   - The platform bus is an internal framework within the kernel that manages the relationships between platform drivers and platform devices.
   - It facilitates the probing and removal of platform devices by their associated drivers.

3. **Initialization and Interaction:**
   - When the kernel initializes, it detects the presence of the `gpio17` platform device based on the Device Tree description.
   - It then calls the `my_gpio_probe` function from the `my-gpio-driver` platform driver to initialize GPIO pin 17.
   - The platform driver abstracts hardware-specific details and provides a standardized interface for GPIO pin control.

In summary, the platform bus, platform drivers, and platform devices work together to enable standardized hardware control in the Linux kernel. They use mechanisms like the Device Tree and driver registration to manage hardware components efficiently. In this example, we demonstrated how these components are used to control a GPIO pin on a Raspberry Pi-based system.
**Relationship Summary:**
- Platform devices are hardware components integrated into the platform and described in the Device Tree or board files.
- Platform drivers are device drivers designed to work with specific platform devices.
- The platform bus serves as a framework that connects platform drivers with platform devices, facilitating the discovery, initialization, and management of platform-specific hardware.

In essence, platform drivers and platform devices rely on the platform bus to create a standardized and modular system for hardware support in the Linux kernel. This architecture allows for hardware abstraction, code reusability, and compatibility across different embedded systems while allowing device drivers to be tailored to the specific hardware components of each platform.

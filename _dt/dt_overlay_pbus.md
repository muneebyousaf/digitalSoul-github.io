---
title: Device Tree, Device Tree overlay and platform bus 
---
# Device Tree, Device Tree overlay and platform bus 
## Device Tree Overlays (DTOs) 
Device Tree Overlays (DTOs) are a mechanism in the Device Tree framework used in embedded systems and single-board computers like the Raspberry Pi to dynamically modify and extend the hardware description provided by the base Device Tree. Device Trees themselves are data structures that describe the hardware components and their connections on a particular platform or architecture.

Here's how Device Tree Overlays work and how they differ from the base Device Tree:

**1. Base Device Tree:**
   - The base Device Tree is a static description of the hardware components and their properties on a particular platform or system.
   - It is typically compiled into the Linux kernel during the build process.
   - The base Device Tree provides a foundation for hardware initialization and driver binding when the system boots.
   - Any modifications or additions to the hardware description require recompiling and reinstalling the kernel.

**2. Device Tree Overlays:**
   - Device Tree Overlays are a way to make dynamic changes to the base Device Tree without recompiling the kernel.
   - They are often used for adding or modifying hardware components on the fly, such as adding sensors or disabling unused peripherals.
   - Device Tree Overlays are typically compiled separately from the kernel and are loaded into the running kernel at runtime.
   - They allow for hardware configuration to be customized without modifying the kernel source code or recompiling the entire kernel.

Here's a simplified workflow for using Device Tree Overlays:

1. Create a Device Tree Overlay (DTO) file: This file describes the changes or additions you want to make to the base Device Tree. It includes details about new devices, device properties, and their connections.

2. Compile the DTO: Use the Device Tree Compiler (dtc) to compile the DTO file into a binary Device Tree Overlay (DTBO) file.

3. Load the DTBO: Use a tool like `dtoverlay` on systems like the Raspberry Pi to load the DTBO into the running kernel. This dynamically extends the hardware description.

4. Unload or remove the DTBO: If necessary, you can unload or remove a Device Tree Overlay, reverting the hardware configuration to its previous state.

Device Tree Overlays are useful for scenarios where hardware configurations need to be flexible and adaptable without the need for kernel recompilation. This makes them particularly suitable for embedded systems and single-board computers, where the hardware configuration may vary depending on the specific use case or hardware peripherals added or removed.

In summary, Device Tree Overlays are a way to extend or modify the base Device Tree at runtime, allowing for dynamic hardware configuration changes without kernel recompilation, making them a valuable tool for embedded and IoT (Internet of Things) projects.
## Example
let's illustrate the concept of Device Tree Overlays (DTOs) and how they differ from the base Device Tree with a simple example using the Raspberry Pi GPIO pins.

**Base Device Tree:**

Imagine you have a Raspberry Pi with a base Device Tree that describes its hardware configuration. By default, the base Device Tree doesn't know about any specific GPIO pins you want to use.

**Device Tree Overlay:**

Now, let's say you want to use GPIO pin 17 for controlling an LED. You can create a Device Tree Overlay (DTO) to specify this configuration.

**Base Device Tree (base.dts):**

```dts
/dts-v1/;
/plugin/;

/ {
    compatible = "raspberrypi,3-model-b";
    /* Base hardware description for the Raspberry Pi 3 Model B */
    fragment@0 {
        target = <&gpio>;
        __overlay__ {
            gpio17: gpio@17 {
                /* Description of GPIO pin 17 */
                compatible = "raspberrypi,gpio";
                gpio-line-names = "gpio17";
            };
        };
    };
};
```

**Device Tree Overlay (overlay.dts):**

```dts
/dts-v1/;
/plugin/;

/ {
    fragment@0 {
        target-path = "/";
        __overlay__ {
            led_pin: led_pin {
                /* Description of LED connected to GPIO pin 17 */
                compatible = "gpio-leds";
                gpios = <&gpio17 0>; /* GPIO pin 17, initial state off (0) */
                default-state = "off";
                linux,default-trigger = "heartbeat";
            };
        };
    };
};
```

In this example:

1. The `base.dts` is the base Device Tree for the Raspberry Pi 3 Model B. It doesn't contain any specific information about GPIO pin 17 or LED control.

2. The `overlay.dts` is a Device Tree Overlay that describes the LED connected to GPIO pin 17. It adds the necessary details for the LED, such as GPIO configuration and default behavior.

**Compiling and Applying the DTO:**

You would compile the Device Tree Overlay (`overlay.dts`) into a Device Tree Overlay Binary (DTBO) file using the Device Tree Compiler (dtc):

```bash
dtc -I dts -O dtb -o overlay.dtbo overlay.dts
```

Then, you would load the DTBO into the running kernel:

```bash
dtoverlay overlay.dtbo
```

**Effect:**

After applying the Device Tree Overlay, the kernel now knows about the LED connected to GPIO pin 17. You can interact with it using the appropriate sysfs nodes:

- `/sys/class/gpio/gpio17/`: Control GPIO pin 17 directly.
- `/sys/class/leds/led_pin/`: Control the LED, which is now recognized as a GPIO LED.

This example demonstrates how Device Tree Overlays extend the base Device Tree dynamically. Without recompiling the kernel, you've added specific hardware descriptions for GPIO pin 17 and the connected LED, allowing you to control the LED through the Linux GPIO and LED subsystems.
## Target Paths    Device Tree overlays  
Target paths in Device Trees specify where the modifications in a Device Tree Overlay (DTO) should be applied within the base Device Tree (DTB). Understanding different target paths is crucial for accurately and selectively extending or overriding the hardware configuration. Let's explore different target paths with examples:

**Base Device Tree (DTB):**

Imagine a base Device Tree that describes a simple ARM-based system with components like the CPU, memory, and UART:

```dts
/dts-v1/;
/plugin/;

/ {
    compatible = "arm,my-system";
    cpu {
        compatible = "arm,cortex-a53";
        /* CPU-specific properties */
    };
    memory {
        /* Memory configuration */
    };
    uart0 {
        compatible = "ns16550a";
        /* UART configuration */
    };
};
```

**Device Tree Overlay (DTO) Examples:**

1. **Targeting a Specific Node:**

   Let's say you want to add a new GPIO controller node under the `cpu` node:

   ```dts
   /dts-v1/;
   /plugin/;

   / {
       fragment@0 {
           target-path = "/cpu";
           __overlay__ {
               gpio0: gpio@0 {
                   compatible = "gpio-controller";
                   /* GPIO controller configuration */
               };
           };
       };
   };
   ```

   In this example, `target-path = "/cpu"` specifies that the modifications within this fragment should be applied under the `cpu` node in the base Device Tree.

2. **Targeting the Root Node:**

   If you want to add a new device at the root level, you can target the root node directly:

   ```dts
   /dts-v1/;
   /plugin/;

   / {
       fragment@0 {
           target-path = "/";
           __overlay__ {
               new_device: new_device {
                   compatible = "my-new-device";
                   /* Configuration for a new device */
               };
           };
       };
   };
   ```

   Here, `target-path = "/"` specifies that the modifications should be applied at the root level of the base Device Tree.

3. **Targeting an Existing Node and Property:**

   You can also modify an existing node and property. For instance, let's change the UART's compatible string:

   ```dts
   /dts-v1/;
   /plugin/;

   / {
       fragment@0 {
           target-path = "/uart0";
           __overlay__ {
               compatible = "my-uart";
           };
       };
   };
   ```

   In this case, `target-path = "/uart0"` specifies that the modification should be applied to the existing `uart0` node's `compatible` property.

4. **Targeting Multiple Nodes:**

   You can target multiple nodes within a single DTO fragment. For example, let's add two GPIO controller nodes under different CPUs:

   ```dts
   /dts-v1/;
   /plugin/;

   / {
       fragment@0 {
           target-path = "/cpu";
           __overlay__ {
               gpio0: gpio@0 {
                   compatible = "gpio-controller";
                   /* GPIO controller configuration */
               };
           };
       };

       fragment@1 {
           target-path = "/cpu";
           __overlay__ {
               gpio1: gpio@1 {
                   compatible = "gpio-controller";
                   /* Another GPIO controller configuration */
               };
           };
       };
   };
   ```

   In this example, two fragments target the same path (`/cpu`) but introduce different GPIO controller nodes.

These examples demonstrate various ways to use target paths within DTOs to extend or modify the base Device Tree. Understanding target paths is essential for precisely configuring your hardware without affecting unrelated parts of the base tree.

## Platform bus and Device trees
In Linux, a "platform bus" refers to a framework or subsystem that provides a standardized way for drivers to interact with and manage hardware devices on a specific platform or architecture. Platform buses abstract the underlying hardware details, making it easier for device drivers to work with various hardware components without having to know the specific hardware details of the platform they are running on.

One well-known example of a platform bus in Linux is the "Device Tree" on ARM-based systems. The Device Tree is a data structure that describes the hardware components and their connections on a particular ARM-based platform. Device Tree allows hardware initialization and driver binding to occur dynamically, depending on the actual hardware configuration, which makes it easier to support a wide range of ARM-based devices with a single Linux kernel.

Here's a simple example to illustrate the concept of a platform bus, specifically using the Device Tree on an ARM-based system:

Let's say you have an ARM-based single-board computer (SBC) that you want to use with Linux. This SBC has an LED connected to GPIO pin 17. To make the LED work with Linux, you need to do the following:

1. Create a Device Tree Overlay (DTO): A Device Tree Overlay is a way to describe additional hardware configurations that can be added or removed dynamically from the base Device Tree. In this case, you create a DTO that describes the LED connected to GPIO pin 17.

   Example DTO (LED-overlay.dts):
   ```dts
   /dts-v1/;
   /plugin/;
   
   / {
       compatible = "my-sbc,my-board";
       fragment@0 {
           target = <&gpio>;
           __overlay__ {
               led_pin: led_pin@17 {
                   compatible = "gpio-leds";
                   gpios = <&gpio0 17 GPIO_ACTIVE_LOW>;
                   default-state = "off";
                   linux,default-trigger = "heartbeat";
               };
           };
       };
   };
   ```

2. Compile the Device Tree Overlay: Use the Device Tree Compiler (dtc) to compile the DTO into a binary format that can be loaded into the kernel at runtime.

   ```shell
   dtc -I dts -O dtb -o LED-overlay.dtbo LED-overlay.dts
   ```

3. Load the Device Tree Overlay: Use the `dtoverlay` command to load the Device Tree Overlay into the running kernel.

   ```shell
   dtoverlay LED-overlay.dtbo
   ```

4. Interact with the LED: Now that the Device Tree Overlay is loaded, the kernel knows about the LED connected to GPIO pin 17. You can interact with the LED by writing to the appropriate sysfs node.

   ```shell
   # Turn the LED on
   echo 1 > /sys/class/leds/led_pin/brightness

   # Turn the LED off
   echo 0 > /sys/class/leds/led_pin/brightness
   ```

In this example, the Device Tree serves as the platform bus framework, abstracting the hardware details of the ARM-based SBC and providing a standardized way for device drivers (in this case, the GPIO and LED drivers) to interact with the hardware. The Device Tree Overlay allows you to dynamically configure and add hardware components to the system without modifying the kernel source code.

This is just one example, and there are other platform buses and mechanisms for hardware abstraction in Linux, depending on the specific hardware and architecture you're working with.
## Note

The `fragment@0` in the base Device Tree (DTB) and the `fragment@0` in the Device Tree Overlay (DTO) are separate and unrelated constructs. They serve different purposes and exist in different contexts within the Device Tree ecosystem.

1. **`fragment@0` in the Base Device Tree (DTB)**: In the context of the base Device Tree, `fragment@0` doesn't have any specific meaning or significance. It appears to be part of a hypothetical example or context you might encounter, but it's not a standard or commonly used construct in the base Device Tree. The base Device Tree describes the hardware configuration of a particular platform, and it doesn't typically contain `fragment@0` entries.

2. **`fragment@0` in the Device Tree Overlay (DTO)**: In the context of a Device Tree Overlay, `fragment@0` is a commonly used construct. It represents a fragment within the overlay that specifies a set of hardware modifications or additions to the base Device Tree. The `fragment@0` in the DTO is used to encapsulate changes made by the overlay and defines the target path within the base tree where these changes should be applied.

   - `target-path` in the DTO's `fragment@0` specifies where in the base Device Tree the overlay's modifications should take effect.
   - `__overlay__` in the DTO's `fragment@0` encapsulates the changes made by the overlay within this fragment.

In summary, while `fragment@0` in the DTO is a standard construct used to organize and specify hardware modifications within an overlay, `fragment@0` in the base Device Tree is not part of the standard Device Tree syntax and doesn't have a specific role within the base tree. The two are unrelated and serve different purposes within the Device Tree infrastructure.

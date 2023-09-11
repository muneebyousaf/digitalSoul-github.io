---
title:   Device Model (Device Driver)
---

## The Linux Kernel Device Model

The Linux kernel device model is organized around three main data structures:

1. **The `struct bus_type` Structure:**
   - The `struct bus_type` structure represents a specific type of bus within the kernel, such as USB, PCI, I2C, or others.
   - This structure defines characteristics and operations specific to that bus type.
   - For example, in the case of USB:

     ```c
     static struct bus_type usb_bus_type = {
         .name = "usb",
         .dev_name = "usb",
         .match = usb_device_match,
         .uevent = usb_uevent,
     };
     ```

   - Here, we define the `usb_bus_type` structure for the USB bus. It includes fields like the bus name and callbacks for device matching and event handling.

2. **The `struct device_driver` Structure:**
   - The `struct device_driver` structure represents a driver capable of handling devices on a specific bus. Each driver is associated with one or more devices of that type.
   - For example, in the context of a USB storage driver:

     ```c
     static struct device_driver usb_storage_driver = {
         .name = "usb-storage",
         .bus = &usb_bus_type,
         .probe = usb_storage_probe,
         .remove = usb_storage_remove,
     };
     ```

   - Here, `usb_storage_driver` is a USB storage driver that has a name, specifies the bus it belongs to (USB), and defines probe and removal callbacks.

3. **The `struct device` Structure:**
   - The `struct device` structure represents an individual device connected to a specific bus. Each device is associated with a device driver that manages it.
   - For example, in the context of a USB device:

     ```c
     static struct usb_device my_usb_device = {
         .dev = {
             .bus = &usb_bus_type,
             .parent = &usb_device_root,
             .driver = &my_usb_driver,
             .kobj = &my_usb_device_kobj,
         },
         .idVendor = USB_VENDOR_ID_MYDEVICE,
         .idProduct = USB_PRODUCT_ID_MYDEVICE,
     };
     ```

   - Here, `my_usb_device` represents a USB device. It includes information about the USB bus it's connected to, its driver, and its vendor and product IDs.

### Inheritance and Specialization

In the Linux kernel, inheritance is used to create more specialized versions of `struct device_driver` and `struct device` for each bus subsystem. These specialized structures include additional fields and functions specific to that subsystem.

For instance, within the USB subsystem:

- Specialized `struct device_driver` and `struct device` structures are used for USB drivers and devices.
- These structures inherit from generic versions but include USB-specific details.
- This specialization ensures that USB drivers and devices adhere to USB standards and protocols.



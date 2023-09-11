---
title: Device Model data structures
---


## Device Model data structures with example

1. **The `struct bus_type` Structure:**
   - In the case of a USB device, the `struct bus_type` representing the USB bus might look like this:
     ```c
     struct bus_type usb_bus_type = {
         .name = "usb",
         .match = usb_device_match,
     };
     ```
   - Here, we define the `usb_bus_type` structure with the name "usb" and specify a matching function (`usb_device_match`) that helps identify USB devices on the bus.

2. **The `struct device_driver` Structure:**
   - Consider a USB storage driver as an example. It would have a `struct device_driver` like this:
     ```c
     struct usb_driver usb_storage_driver = {
         .name = "usb-storage",
         .probe = usb_storage_probe,
         .disconnect = usb_storage_disconnect,
     };
     ```
   - This `usb_storage_driver` structure represents the USB storage driver, providing its name ("usb-storage") and callback functions for device probing (`usb_storage_probe`) and disconnection (`usb_storage_disconnect`).

3. **The `struct device` Structure:**
   - For a USB device, the `struct device` representing an individual USB device might look like this:
     ```c
     struct usb_device my_usb_device = {
         .bus = &usb_bus_type,
         .parent = &my_usb_hub,
         .descriptor = &usb_device_descriptor,
     };
     ```
   - Here, we define `my_usb_device`, specifying the USB bus it's connected to (`usb_bus_type`), its parent hub (`my_usb_hub`), and its descriptor containing details like vendor and product IDs (`usb_device_descriptor`).

4. **Inheritance and Specialization:**
   - Within the USB subsystem, specialized versions of `struct device_driver` and `struct device` are created for specific USB device types.
   - For example, consider a USB mouse driver:
     ```c
     struct usb_driver usb_mouse_driver = {
         .name = "usb-mouse",
         .probe = usb_mouse_probe,
         .disconnect = usb_mouse_disconnect,
     };
     ```
   - In this case, `usb_mouse_driver` is a specialized `struct device_driver` tailored for USB mice. It inherits common USB-related functions but includes specific functions like `usb_mouse_probe` and `usb_mouse_disconnect` for mouse-related operations.

These examples illustrate how the Linux kernel uses these data structures to organize USB devices and drivers. The `struct bus_type` represents the USB bus, `struct device_driver` represents USB drivers, and `struct device` represents individual USB devices. Specialized versions of these structures are created for different USB device types, enabling the kernel to manage USB devices and drivers effectively.

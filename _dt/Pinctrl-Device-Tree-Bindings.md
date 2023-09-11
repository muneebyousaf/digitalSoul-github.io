---
title: Pinctrl Device Tree Bindings
---
# Pinctrl Device Tree Bindings with Examples

In the context of Device Tree (DT), pin controllers (pinctrl) and their associated client devices play a crucial role in configuring and controlling pins on embedded systems. Let's explore these concepts with detailed examples.

## Introduction

- **Pin Controllers**: These are hardware modules responsible for pin multiplexing and configuring parameters like pull-up/down, tri-state, drive-strength, etc. Each pin controller is represented as a node in the device tree.

- **Client Devices**: These are hardware modules whose signals are affected by pin configuration. Each client device is also represented as a node in the device tree.

- **Pin Configurations**: Certain pin controllers must set up specific pin configurations for client devices to operate correctly. Some client devices need static configurations, while others require dynamic configuration during runtime.

## Pin Controller Example

Suppose we have a pin controller for an LED module with various configuration states:

```device-tree
led_controller: led-controller {
    compatible = "example,led-controller";
    #gpio-cells = <2>;
    pinctrl-names = "active", "idle";
    pinctrl-0 = <&led_active_cfg>;
    pinctrl-1 = <&led_idle_cfg>;
};

led_active_cfg: led-active-cfg {
    gpio-hog;
    gpios = <&gpio0 0 GPIO_ACTIVE_HIGH>;
};

led_idle_cfg: led-idle-cfg {
    gpio-hog;
    gpios = <&gpio0 1 GPIO_ACTIVE_HIGH>;
};
```
In this example, we define a `led_controller` node representing the pin controller for an LED module. We have two pin configurations: `led_active_cfg` and `led_idle_cfg`. These configurations are assigned names "active" and "idle" respectively, and they set up GPIO pins accordingly.

## Pin Client Device Example

Now, let's consider a client device, a button, which depends on the pin controller's configurations:
``` device
button: button {
    compatible = "example,button";
    pinctrl-names = "active";
    pinctrl-0 = <&button_active_cfg>;
};

button_active_cfg: button-active-cfg {
    gpio-hog;
    gpios = <&gpio1 0 GPIO_ACTIVE_LOW>;
};
```
In this example, we define a `button` node representing a button as a client device. It depends on the "active" pin configuration provided by the pin controller. The button uses GPIO pin 0 from `gpio1` and specifies GPIO_ACTIVE_LOW as the desired configuration
 
## Combining Configurations

Consider a scenario where a more complex configuration is needed, combining multiple pin controllers for a touchscreen controller:

``` device
touchscreen: touchscreen {
    compatible = "example,touchscreen";
    pinctrl-names = "active";
    pinctrl-0 = <&ts_active_cfg>;
};

ts_active_cfg: ts-active-cfg {
    pinctrl-0 = <&ts_gpio_cfg &ts_irq_cfg>;
};

ts_gpio_cfg: ts-gpio-cfg {
    gpio-hog;
    gpios = <&gpio2 0 GPIO_ACTIVE_HIGH>;
};

ts_irq_cfg: ts-irq-cfg {
    gpio-hog;
    gpios = <&gpio3 2 GPIO_ACTIVE_HIGH>;
};
```

In this example, we have a `touchscreen` client device depending on an "active" pin configuration. The "active" configuration includes two sub-configurations, `ts_gpio_cfg` and `ts_irq_cfg`. These sub-configurations set up GPIO pins for the touchscreen controller and its interrupt.

These examples demonstrate how pin controllers and pin client devices are represented in Device Tree, making it easier to configure and control pins for various hardware modules in embedded systems.

---
title: Defining pinctrl configurations
---

**PinCtrl Configurations in Device Tree**

1.  _Different PinCtrl Configurations Defined as Child Nodes:_
    
    -   The Device Tree hierarchy includes main PinCtrl devices with child nodes representing different pin configurations.
    
    **Example:**
    

    
    ```DT
    pinctrl: pinctrl@f8000000 {
        compatible = "pinctrl-generic";
        #address-cells = <1>;
        #size-cells = <0>;
        
        /* Define different pin configurations as child nodes */
        state_default: default {
            /* Pin configuration settings for the default state */
            /* ... */
        };
    
        state_gpio: gpio {
            /* Pin configuration settings for the GPIO state */
            /* ... */
        };
    };
    ```
    
2.  _Defining Configurations at SoC or Board Level:_
    
    -   Pin configurations can be defined at different levels:
        -   At the SoC level in a `.dtsi` file for shared configurations.
        -   At the board level in a `.dts` file for board-specific configurations.
    
    **Example (SoC Level - .dtsi File):**
    

``` DT    
    /* PinCtrl node defined in SoC's .dtsi file */
    pinctrl: pinctrl@f8000000 {
        /* Pin configurations for multiple boards sharing the same SoC */
        /* ... */
    };
  ```
  
    
    
 **Example (Board Level - .dts File):**
    

 ```DT   
    `/* PinCtrl node defined in a board-specific .dts file */
    pinctrl: pinctrl@f8000000 {
        /* Pin configurations specific to this board */
        /* ... */
    };` 
```
    
3.  _Using Phandles to Reference Pin Configurations:_
    
    -   Consumer devices specify which pin configuration to use by referencing it with a unique phandle.
    
    **Example (Consumer Device - .dts File):**
 
```DT    
    gpio_consumer: gpio_consumer {
     compatible = "example,consumer";
        pinctrl-names = "default";
        pinctrl-0 = <&state_gpio>; /* Reference the GPIO pin configuration */
        /* ... */
    }; 
 ```
 
    



<p align="center">
  <img src="/pics/smrt_clk.jpg" alt="BatBone" width="400">
</p>

# SMRT-CLK
Smart clock / dashboard based on the Raspberry Pi. 

## Building the SMRT CLK
This section will cover the materials and steps needed to build the SMRT CLK.

### Order the PCB
The first step is to order the PCB. You can use the schematics to order the board from your favorite PCB manufacturer or you can use the link below to order it from OSH Park. 
<a href="https://oshpark.com/shared_projects/3NsGFRa8"><img src="https://oshpark.com/assets/badge-5b7ec47045b78aef6eb9d83b3bac6b1920de805e9a0c227658eac6e19a045b9c.png" alt="Order from OSH Park"></img></a>

## Setting Up the SMRT CLK
There are just a few files that need to be copied to the Pi itself and then some of them need to be compiled. These files are located in the */distro/* folder. We will start first with the device tree overlays which need to be transfered to the Pi and compiled.

### Device Tree Overlays
I have written some device tree settings that need to be compiled into device tree overlays and placed in the proper folder. The overlays do the following:
1. *dpi565.dts* - This sets the GPIO that we use for the screen to their alternate function which is DPI output.
2. *pullup.dts* - This sets the pullup resistors for the GPIO pins that we are using for I2C. We are using I2C to communicate with the touch screen controller.
3. *touchscreen.dts* - This sets the driver to use with the I2C touch controller. It also registers a GPIO pin to use as a hardware interrupt for the touch controller.
The device tree overlays should be compiled with the following commands:
```
sudo dtc -I dts -O dtb -o /boot/overlays/dpi565.dtbo dpi565.dts
sudo dtc -I dts -O dtb -o /boot/overlays/pullup.dtbo pullup.dts
sudo dtc -I dts -O dtb -o /boot/overlays/touchscreen.dtbo touchscreen.dts
```

### Boot Configuration
I also have included a *config.txt* that should replace the existing one in the Raspbian */boot/* folder. The purpose of the file configuration file is to tell Raspbian to load the device tree overlays we compiled. Additionally, the configuration file has the settings (resolution, timings, etc...) for the LCD display.

### Touch Screen Calibration
Once the device tree overlays and boot configuration are properly setup, the display and touch screen should work after rebooting the Pi. The final step is to calibrate the touch screen. I found these calibration values through trial and error. Eventually I would like to write a script that does the calibration by having the user touch calibration points on the screen. The screen can be calibrated with the following commands:
```
DISPLAY=:0 xinput set-prop 'EP0790M09' 'Coordinate Transformation Matrix' 1 0 -1 0 1 0 0 0 1
DISPLAY=:0 xinput set-prop 'EP0790M09' 'libinput Calibration Matrix' 6.5 0 1 0 10 0 0 0 1
```
This calibration only lasts as long as the Pi is on. To make the calibration permanent, the commands need to be placed in a script and executed once the Pi is on and the device tree overlays have been loaded. There are various ways to do this depending on your application and version of Raspbian.

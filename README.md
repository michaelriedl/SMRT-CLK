# SMRT-CLK
<p align="center">
<img src="/pics/smrt_clk.jpg" alt="BatBone" width="400">
</p>
Smart clock / dashboard based on the Raspberry Pi. 

## Setting Up the SMRT CLK
There are just a few files that need to be copied to the Pi itself and then some of them need to be compiled. These files are located in the */distro/* folder. We will start first with the device tree overlays which need to be transfered to the Pi and compiled.

### Device Tree Overlays
I have written some device tree settings that need to be compiled into device tree overlays and placed in the proper folder. The overlays do the following:
1. *dpi565.dts* - This sets the GPIO that we use for the screen to their alternate function which is DPI output.
2. *pullup.dts* - This sets the pullup resistors for the GPIO pins that we are using for I2C. We are using I2C to communicate with the touch screen controller.
3. *touchscreen.dts* - This sets the driver to use with the I2C touch controller. It also registers a GPIO pin to use as a hardware interrupt for the touch controller.
The device tree overlays should be compiled with the following commands:
'''
sudo dtc -I dts -O dtb -o /boot/overlays/dpi565.dtbo dpi565.dts
sudo dtc -I dts -O dtb -o /boot/overlays/pullup.dtbo pullup.dts
sudo dtc -I dts -O dtb -o /boot/overlays/touchscreen.dtbo touchscreen.dts
'''

### Boot Configuration
I also have included a *config.txt* that should replace the existing one in the Raspian */boot/* folder. The purpose of the file configuration file is to tell Raspbian to load the device tree overlays we compiled. Additionally, the configuration file has the settings (resolution, timings, etc...) for the LCD display.

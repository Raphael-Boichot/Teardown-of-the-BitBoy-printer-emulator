# Teardown of the BitBoy printer emulator

Too cheap to buy a 99€ + shipping Game Boy Printer emulator ? Too lazy to remove two Phillips screws ? Here are some images of what's inside this [toy for wealthy nerds](https://gameboyphoto.bigcartel.com/) ! The development story can be found [here](/Datasheets/BitBoy_Project_Development_Brief_V2.0.pdf). The device is sadly completely closed (not open source). I got mine second hand for half the price around 2021, which is still way too expensive to my own taste but this was for scientific purpose.

Let's see what's inside the beast.

## General aspect
![](/Images/BitBoy_1.png)
The device uses a neat molded case. It fires by connecting a serial cable with Game Boy on so it's probably using an interrupt reading CLOCK pin (HIGH by default) to wake-up the main chip from deep sleep. It comprises a discrete hole on the back side to reset the chip as it can (quite oftenly) freeze or crash.

## Upper side
![](/Images/BitBoy_2.png)
The device uses a 500 mAh battery which is virtually never depleted as the current draw is nearly nothing despite powering a RTC and the SD card when writing. It can print thousands of images on a single charge (I never managed to go out of charge in endurance tests). Access to SD card is surprisingly sluggish in the other hand. The device uses flux control to manage this slow access. This means that it can access SD card without detaching the interrupt on CLOCK which is quite impressive. The red socket on this side of the PCB is probably the flashing port for the microcontroller. There is no way to update the firmware for a normal user.

## Upper side without battery
![](/Images/BitBoy_3.png)
The version I have is a 2.0. The serial socket is a GBA one, these are the most cheap and common to find. This is of course compatible with a GBC serial cable. A big giant SD card slot was chosen, which is unusual even in 2020, date of release for this revision.

## Lower side showing chips
![](/Images/BitBoy_4.png)
The main chip is a PIC24FJ128GA106, a 54 pins 16 bits microcontroller with 128 kB flash and 16 kB SRAM. It runs at 32 MHz. Quite powerfull compared to an Arduino but most of all, very low power consumption. The battery is managed by a MAX1811 USB powered lithium charger. The RTC chip is a I²C MCP79400 from Microchip. I do not see any obvious bus transciever but some pins are 5.5V tolerant so the PIC can probably directly eat 5V signals from the Game Boy. I cannot identify U2 (marking 0B2N), U3 (marking 332C) and Q1 (marking M03) for sure at the moment I wrote this. One of them must be a 3.3V regulator for the SD card. These tiny chips are probably involved in power management.

Let's be honest, it's nicely engineered.

## Some identified flaws after my own tests

The device is compatible with most of the western games including Pokémons, Zelda, Game Boy Camera, etc. It supports compressed protocol and custom palettes. The BitBoy probably uses time out and paper feed information to detect end of image automatically (plus some other tricks like counting packets to assemble images in particular games). Some common games may crash the device when a time out is detected before a feed paper command (so typically out of a Mary-Kate and Ashley: Pocket Planner context).

Japanese only games are more or less managed as long as the protocol is sufficiently "western games like". Most homebrews just don't work as they push the protocol to its limit. Photo! is supported in normal speed only, double speed mode + batch printing crashes the device.

My main gripes (I mean out of the outrageous price) are the native 1x BMP image format that obliges to use an additional image converter for online publication (BMP is not natively supported on most platforms and 4x upscaling is a very minimal requirement to see something on a modern display), and most of all, the pain in the arse to reboot the device. Because yes, the devices crashes (more precisely, freezes) sometimes, and unbricking it requires to find a needle to push the reset button through the tiny hole designed for this purpose (or open the shell with a screwdriver). You're far from home in that case, you're left with a brick (own experience). The lack of easy to access reboot/ on-off function is a very big design flaw to me.

The fact that the device is completely closed makes it impossible to improve. It is now quite outdated compared to DIY open access counterparts which integrates decent file format, more printing options, wifi support, sometimes even image post-processing and full game compatibility.

To be continued (or not).

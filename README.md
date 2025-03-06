# Teardown of the BitBoy printer emulator

Too cheap to buy a 99€ + shipping Game Boy Printer emulator ? Too lazy to remove two Phillips screws ? Here are some images of what's inside this [toy for wealthy nerds](https://gameboyphoto.bigcartel.com/) ! The development story can be found [here](/Datasheets/BitBoy_Project_Development_Brief_V2.0.pdf). The device is sadly completely closed (not open source). I got mine second hand for half the price around 2021, which is still too expensive to my own taste but this was for science.

## General aspect
![](/Images/BitBoy_1.png)
The device uses a neat molded case. It fires by connecting a serial cable with Game Boy on so it's probably using an interrupt reading CLOCK pin (HIGH by default) to wake-up the main chip from deep sleep. It comprises a discrete hole on the back side to reset the chip as it can (quite oftenly) freeze or crash.

## Upper side
![](/Images/BitBoy_2.png)
The device uses a 500 mAh battery which is virtually never depleted as the current draw is nearly nothing despite powering a RTC and the SD card when writing. It can print thousands of images on a single charge (I never managed to go out of charge in endurance tests). Access to SD card is surprisingly sluggish in the other hand. The device uses flux control to manage this slow access. This means that it can access SD card without detaching the interrupt on CLOCK which is quite impressive. The red socket on this side of the PCB is probably the flashing port for the microcontroller. There is not way to update the firmware for a normal user.

## Upper side without battery
![](/Images/BitBoy_3.png)
The version I have is a 2.0. The serial socket is a GBA one, these are the most cheap and common to find. This is of course compatible with a GBC serial cable. A big giant SD card slot was chosen, which is unusual even in 2020, date of release for this revision.

## Lower side showing chips
![](/Images/BitBoy_4.png)
The main chip is a PIC24FJ128GA106, a 54 pins 16 bits microcontroller with 128 kB flash and 16 kB SRAM. It runs at 32 MHz. Quite powerfull compared to an Arduino but most of all, very low power consumption. The battery is managed by a MAX1811 USB powered lithium charger. The RTC chip is a I²C MCP79400 from Microchip. I do not see any obvious bus transciever but some pins are 5.5V tolerant so the PIC can probably directly eat 5V signals from the Game Boy. I cannot identify U2 (marking 0B2N), U3 (marking 332C) and Q1 (marking M03) for sure at the moment I wrote this. One of them must be a 3.3V regulator for the SD card. These tiny chips are probably involved in power management.

## Some identified flaws after my own tests

The device is compatible with most of the western games including Pokémons, Zelda, Game Boy Camera, etc. The BitBoy uses both time out or paper feed information to detect end of image, depending on the context. I suspect that some particular strategies have been tuned for particular games by counting packets or detecting some particular checksums (like Mary-Kate and Ashley: Pocket Planner which is supported without sending feed paper signal). It overall supports compressed protocol and custom palettes. Some common games may crash the device for not that obvious reasons, in general when a time out is detected before the feed paper command (so out of a Mary-Kate and Ashley: Pocket Planner context typically).

Japanese only games are more or less managed as long as the protocol is sufficiently "Game Boy Camera like". Most homebrews just don't work as they push the protocol to its limit. Photo! is supported in normal speed only.

In brief, the BitBoy is very adapated for the Game Boy Camera and very common games, prone to crash in the other cases. This anyway must fit with 99.9% of its use as printer emulator.

The lack of update makes it impossible to improve and it is now quite outdated compared to 2025 open access counterparts which integrates some undocumented tricks of the Game Boy Printer protocol.

To be continued (or not).

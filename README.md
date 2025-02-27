# Teardown of the BitBoy printer emulator

Too cheap to buy a 99€ + shipping Game Boy Printer emulator ? Too lazy to remove two Phillips screws ? Here are some images of what's inside this [toy for wealthy nerds](https://gameboyphoto.bigcartel.com/) ! The development story can be found [here](/Datasheets/BitBoy_Project_Development_Brief_V2.0.pdf). The device is sadly completely closed. I got mine second hand for half the price around 2021, which is still too expensive to my own taste but this was for science.

## General aspect
![](/Images/BitBoy_1.png)
The device uses a neat molded case. It fires by connecting a serial cable with Game Boy on so it's probably using an interrupt on CLOCK pin (HIGH by default) to wake-up the main chip from deep sleep. It comprises a discrete hole to reset the chip as it can (quite oftenly) freeze or crash.

## Upper side
![](/Images/BitBoy_2.png)
It uses a 500 mAh battery which is virtually never depleted as the device consumes nothing despite using a RTC and writing to SD card. It can print thousands of images on a single charge (I never managed to go out of charge). Access to SD card is surprisingly sluggish. The device uses flux control to manage this slow access to SD card. The red socket is probably the flashing port for the microcontroller.

## Upper side without battery
![](/Images/BitBoy_3.png)
The version I have is a 2.0. The serial socket is a GBA one, these are the most cheap and common to find. This is of course compatible with a GBC serial cable.

## Lower side showing chips
![](/Images/BitBoy_4.png)
The main chip is a PIC24FJ128GA106, a 54 pins 16 bits microcontroller with 128 kB flash and 16 kB SRAM. It runs at 32 MHz. Quite powerfull compared to an Arduino but most of all, very low power consumption. The battery is managed by a MAX1811 USB powered lithium charger. The RTC chip is a I²C MCP79400 from Microchip. I do not see any obvious bus transciever but some pins are 5.5V tolerant so the PIC can probably directly eat 5V signals from the Game Boy. I cannot identify U2 (marking 0B2N), U3 (marking 332C) and Q1 (marking M03) for sure at the moment I wrote this.

## Some identified flaws

I'm not working for Bigcartel and I won't debug the device for free (my hourly rate is too expensive for them anyway). But the device is compatible with most of the Western games including Pokémons, Zelda, Game Boy Camera, etc. The BitBoy uses both time out or paper feed information to detect end of image, depending on the games. I suspect that some particular strategies have been tuned for particular games (like Mary-Kate and Ashley: Pocket Planner which is supported without sending feed paper signal). It overall supports compressed protocol and custom palettes. Common games may crash the printer for no obvious reasons.

Japanese only games are more or less managed as long as the protocol is sufficiently "Game Boy Camera like". Most homebrews just don't work as they push the protocol to its limit. Photo! is supported in normal speed only.

To be continued (or not)

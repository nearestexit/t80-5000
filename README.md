# T80-5000 & T80-5700

[QMK](https://github.com/qmk/qmk_firmware) Compatible Open Hardware USB-C PCBs for the vintage [Cherry G80-5000](https://deskthority.net/wiki/Cherry_G80-5000) split ergonomic keyboard and its companion numpad, the G80-5700.



## Overview
The original G80-5000 has two halves which are attached together with a spring, similar to the IBM M15 and the Apple Adjustable Keyboard. They cannot be detached from each other, and the right half must always act as the host.
The original G80-5700 companion numpad can only function when plugged in to the right half of the G80-5000.

This project brings the G80-5000 & 5700 into the 21st century, with all three pieces able to function independently.
There are four PCBs included in this project: the T80-5000 Left, T80-5000 Right, T80-5000 Daughterboard, and T80-5700.
The T80-5000 Left & Right work with QMK's [split keyboard](https://docs.qmk.fm/features/split_keyboard) functionality, and the T80-5700 functions as an independent keyboard which can be passed through the right half's USB hub.

Production files are included for production at [JLCPCB](https://jlcpcb.com/).

![Overview](https://github.com/nearestexit/t80-5000/blob/main/documentation/DSC03446.JPG)


## Features
 - USB-C in several configuration options, including internally attached and externally detachable
 - Integrated USB2.0 hub intended for T80-5700 numpad to support passthrough like the original G80-5700
 - [RP2040](https://www.raspberrypi.com/products/rp2040/specifications/) MCU(s) supported by the open-source QMK firmware project

![PCB](https://github.com/nearestexit/t80-5000/blob/main/documentation/allcombo.png)

## Supported layouts
 - ISO and ANSI
 - Split backspace
 - 5x4 ortholinear numpad
 - Stepped and centered caps lock
   
![T80-5000 and 5700 layout options](https://github.com/nearestexit/t80-5000/blob/main/documentation/KLE.png?raw=true)

## Configuration options
 - The left and right halves can function independently or in a primary/secondary configuration.
 - The left half's USB-C connector is only accessible internally.
 - The right half's USB-C connector takes advantage of the cutout for the original 5-pin mini DIN to be externally detachable. It is recommended to use the right half as the host.
 - The left and right half can be connected by an internally attached TRRS cable or 4-pin JST-SH cable.
 - The JST-SH cable is small enough to fit through a replacement spring.
 - The daughterboard fits in the right half, leverages the same cutout in the case, and serves as the downstream facing USB-C port for the integrated USB hub. It is connected to the right half with a 4-pin JST-SH cable.
 - The T80-5700 has an internally detachable USB-C connector and functions as an independent USB device.

![USB port pictures](https://github.com/nearestexit/t80-5000/blob/main/documentation/DSC03448.JPG)
![Combo picture](https://github.com/nearestexit/t80-5000/blob/main/documentation/misccombo.png)

## JLCPCB Ordering Information
1. On jlcpcb.com, click "order now" and "add gerber file"
2. Navigate to the directory with the gerber.zip - for example, t80-5000-left\jlcpcb\production_files
3. Wait for the gerber to upload.
4. Select the number of PCBs you wish to build.
5. Make sure PCB thickness is set to 1.6mm.
6. Make sure Via covering is set to "tented".
7. Make sure Min via hole size/diameter is set to "0.3mm"
8. For "Mark on PCB" select "Order Number (Specify Position)"
9. Turn the "PCB Assembly" option on
10. Make sure PCBA type is set to "standard"
11. For "Assembly side", if you are ordering the daughterboard select "top side", otherwise select "bottom side"
12. Click "next", and then "next" again
13. Click "Add BOM File" and select the BOM csv - for example, BOM-T80-5000-left.csv
14. Click "Add CPL File" and select the CPL csv - for example, CPL-T80-5000-left.csv
15. Click "Process BOM and CPL"
16. Make sure all parts are in stock - if any are not in stock, you will need to find replacements
17. If you wish to purchase and solder the TRRS sockets separately (recommended to save money), unselect "J2" in this list
18. **IMPORTANT:** Click "Bottom" to view the bottom side of the PCB, and begin inspecting the component placements. You **WILL** need to correct some component rotations. If you are not sure at this step, do not continue!
19. Click "next" and finish checking out.

When I ordered the V0.1 prototype on 11/8/2024, The total price was $207.63 total for Qty. 5 of all PCBs (left, right, daughterboard, and 5700) plus shipping, or roughly $52.71 per set of Left, Right, Daughterboard, and 5700.

![JLCPCB order](https://github.com/nearestexit/t80-5000/blob/main/documentation/JLCPCB%20order.png)

## Additional Components:
- 1x 1:1 ~50mm 4-pin JST-SH cable to connect Left PCB to daughterboard
  - **IMPORTANT:** this must be a 1:1 cable, or else you will short VCC to ground!
  - This means that pin 1 of one connector must be wired to pin 1 of the other connector. If you are unsure, lay the cable flat with no wires crossing - if the connectors on either side are the same way up, it is NOT a 1:1 cable. If they are flipped, it is a 1:1 cable.
  - You can get 1:1 JST-SH cables from many places, but I recommend purchasing a Sparkfun QWIIC starter kit like [this](https://www.amazon.com/dp/B08HQ1VSVL)
- 1x 1:1 ~300mm 4-pin JST-SH cable **OR** TRRS cable to connect Left PCB to Right PCB
  - **IMPORTANT:** do not hotswap these connectors! Plugging or unplugging the TRRS connector with power on will short VCC to ground and potentially damage the MCU.
- 3x LEDs for lock lights
  - The board comes with 300 ohm 1206 package resistors pre-soldered. There are holes to add through-hole resistors if you want to change the resistance.
- ~113 MX-compatible switches (this will vary depending on the layout you choose - my ISO configuration uses 108 switches)
- [PJ-320A](https://www.lcsc.com/product-detail/Audio-Connectors_XKB-Connectivity-PJ-320A_C2884926.html) 3.5mm 4 pin TRRS receptacle (optional) - if you wish to use a TRRS cable and choose not to have it soldered by JLCPCB
- Spring (optional) - An image displaying the approximate measurements of the original spring is included in the documentation folder. I used [these](https://www.amazon.com/dp/B0BRHF1DBL) springs.

## Firmware
- [Vial-QMK](https://get.vial.today/) .uf2 files are provided in the firmware folder.
- To put the keyboard into bootloader mode to flash the firmware:
1. Unplug the USB cable
2. Short the BOOT pins with a pair of tweezers
3. Plug the USB cable in
4. Stop shorting the BOOT pins.
- The RP2040 will then be loaded onto your computer as a USB storage device. Drag in the .uf2 firmware file, and it will reboot.
- The boot pins can be found on the Left side by removing the 1 key, the Right side by removing the 0 key, and the Numpad by removing the * key.
- Once you have flashed the firmware, you can change the keymap from [vial.rocks](https://vial.rocks)

## Other Important Notes
- The components are soldered on to the bottom of this PCB. If you are not careful when putting the keyboard together, you can knock components off the PCB.
- The USB-C receptacles for the right half and the downstream-facing USB hub are close enough together that if the downstream USB-C port is used, cables with thick ends will not work.
- If you do not use the downstream-facing USB-C port, there is no limit to the size of the USB-C cable.
- The USB-Hub will not function if the left side is used as the host.
  

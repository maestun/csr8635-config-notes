# csr8635-config-notes

Configuration changes for **SANWU® 50W+50W TDA7492 CSR8635 Wireless Bluetooth 4.0 Audio Receiver Amplifier Board**
https://www.banggood.com/50W50W-TDA7492-CSR8635-Wireless-Bluetooth-4_0-Audio-Receiver-Amplifier-Board-NE5532-Preamp-p-1109777.html

This board is nice but has a generic bluetooth name, and super annoying sounds at startup.

This explains how to tie this board to a cheapo FT232RL board (such as this one: https://www.banggood.com/FT232RL-FTDI-USB-To-TTL-Serial-Converter-Adapter-Module-For-Arduino-p-917226.html), in order to set some parameters on the CSR8635 bluetooth chip.

### HARDWARE

On the FT232 board, **remove the 3.3 / 5V jumper**. This will let you set the voltage for SPI communications, which is 1.8 volts.
Just below the blue CSR module board, you'll notice some little solder pads, these are the programming headers. Let's solder some wires to those, hook up some resistors, and tie all that to the FT232 board:

(CSR BOARD => FTDI BOARD)
* SPI ENABLE => 10K resistor => 3.3V
* GND => GND
* VCC => 3.3V
* MOSI => 220ohm resistor => RI
* CLK => 220ohm resistor => RTS
* CSB => 220ohm resistor => DTR
* MISO => 220ohm resistor => DSR (might be 'RSD', when counterfeit :p)



Also solder a wire on the pin n°10 of the CSR module: this is a 1.8V output given by the chip. 
This will allow us to use this logic level for SPI communication, without needing logic converters or additional resistors.
**Connect this wire to the VCCIO (might be 'VCC')  pin of the FT232 board.**

It's now time to power up. Unplug the SPI ENABLE wire, connect your FT232 board to a Windows PC using the USB port, then plug the SPI wire. The CSR chip should boot and the LEDs should start to blink.


### SOFTWARE:

On your Windows PC:
* Download zadig-2.3.exe (http://zadig.akeo.ie/), then in Options > List All Devices, select FT232R, then install the **libusbK** driver.
* Install the **BlueSuite 2.5** and **CSRXX_ROM_ConfigTool_3.0.64** programs from CSR (register on their website, or search via the links below :)
* Extract the **lib-win32/usbspi.dll** driver found in the last binary release from https://github.com/lorf/csr-spi-ftdi/releases
* Backup the **usbspi.dll** files found in the CSR tools install folders, then copy the file extracted from above
* Run **PSTool.exe**
* **BEFORE ANYTHING, DUMP YOUR CHIP'S PARAMETERS** by selecting File > Dump (can be a bit long, my file is ~30kB)
* Search for ‘name’ in the text field, then change your device's name
* To get rid of the audio tones, open the dump file and replace every byte under **PSKEY_USR26** by zeroes. Then use the File > Merge... command in PSTool and select your new dump file. Upload can be long !


### SOURCES / THANKS TO
This topic: https://www.eevblog.com/forum/projects/programming-off-the-shelf-csr8635-module/
This repo: https://github.com/lorf/csr-spi-ftdi
This blog post: https://bois083.wordpress.com/2016/10/08/playing-audio-files-with-csr8645-bluetooth-chip/


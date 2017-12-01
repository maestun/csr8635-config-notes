# csr8635-config-notes

Changer config Module CSR8536 / Sanwu 50W+50W

### HARDWARE

Branchement avec un module FT232 cheapos.
**ATTENTION: ENLEVER LE JUMPER 3.3V / 5V DU FT232**
En bas du module BT ils y a comme des test points: c’est les headers de debug. Je compte de gauche à droite, antenne en haut. 

(CSR BOARD => FTDI BOARD)
* SPI ENABLE => 10K => 3.3V
* GND => GND
* VCC => 3.3V
* MOSI => 220R => RI
* CLK => 220R => RTS
* CSB => 220R => DTR
* MISO => 220R => DSR (ou RSD selon les contrefaçons :p)

Ensuite souder un fil sur la pin n°10 du module: c’est un référentiel 1.8V donné par le chip. Brancher le 1.8V sur le VCCIO du FT232 (ou VCC selon les contrefaçons :p)

Débrancher le câble SPI ENABLE du 3.3V, brancher le FT232 en USB à une machine Windows pour faire démarrer les chips, puis rebrancher le câble SPI. Sinon le CSR démarre pas.

### SOFTWARE:

Sur une machine Windows:
* Lancer zadig-2.3.exe (http://zadig.akeo.ie/), Options > List All Devices, sélectionner FT232R, puis installer le driver libusbK
* Installer BlueSuite 2.5 et CSRXX_ROM_ConfigTool_3.0.64
* Récupérer le driver usb ici: https://github.com/lorf/csr-spi-ftdi , fichier lib-win32/usbspi.dll 
* Copier cette DLL à la place de celles présentes dans les dossiers d’install BlueSuite et CSR ConfigTool
* Lancer PSTool.exe
* **FAIRE UN DUMP DES PARAMETRES AVANT TOUT: File > Dump (attention c’est long)**
* Changer ‘name’
* Dans le dump, audio tones alakon => PSKEY_USR26

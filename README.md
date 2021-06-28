# huawei-e3372-unlock

Simple Process To Unlock Huawei E3372h in Linux and Windows 7
This method is for Auth V4 Modems only.  Check your modem whether it is v4

Firmware version applied on 22.200.15.0.210 

## Linux method
### 1. Boot Pin Short
  Install *usb_modeswitch* and *lsusb* if it is not already installed.
  
  Open */etc/usb_modeswitch.conf* under *root* and change *DisableSwitching* variable to *0*
  
  You have to open up your modem and short one of the pin to ground and insert it into usb while holding it in this shorted position. You can short the pin as shown in the figure with a compass or tweezers or small scissor. Hold it in this position for a minute. The led should be off , it should not blink , if it blinks then you have not shorted properly and you have to do it again.
  
  Once you shorted your modem properly you will enter into boot mode , check with *lsusb* and *dmesg* the device configuration should be *12d1:1c01*  and in *dmesg* it should show as *USB CON HUAWEI*  
  
  *ls /dev/ttyUSB\** should give *ttyUSB0*

### 2. Flash Safeloader  
   Flash Safeloader file *usblsafe_e3372h.bin* using *balong-usbdload* after compiling https://github.com/forth32/balong-usbdload
   
   Under root permission *./balong-usbdload -p /dev/ttyUSB0 usblsafe_e3372h.bin* will flash safeloader and the modem will enter into download mode after 10s-15s  led will start to blink  
   
   *lsusb* should show *12d1:1443* and *ls /dev/ttyUSB\** should give 3 ports *ttyUSB0 ttyUSB1 and ttyUSB2*

### 3. Flash Downgrade Firmware
   After you get into download mode you have to flash a downgrade firmware . I have used the huawei official hilink firmware 22.200.5.0.0
   Under root permission flash the firmware using *balongflash* after compiling https://github.com/forth32/balongflash
   *./balong_flash -p /dev/ttyUSB2 E3372h-607_Update_22.200.05.00.00_universal.exe* 
   
### 4. Flash WebUI
   After firmware flash you can flash WebUI Dashboard Software of your choice to control the modem . I have flashed Huawei original version you can choose different operator version as well 
   
   *./balong_flash -p /dev/ttyUSB2 Update_WEBUI_17.100.13.01.03_HILINK.exe*
   
   You might have to try multiple times till the mode changes to *12d1:1442*. When the mode changes to *12d1:1442* you have to change the port to */dev/ttyUSB0* and flash again.
   
   *./balong_flash -p /dev/ttyUSB0 Update_WEBUI_17.100.13.01.03_HILINK.exe*
   
   In this mode *12d1:1442* you will have to flash the firmware again
   
   *./balong_flash -p /dev/ttyUSB0 E3372h-607_Update_22.200.05.00.00_universal.exe* 
   
   Bingo your downgrading and WebUI installation is complete. The modem will be reset and you can see the Wired connection is set up automatically in your Linux Network settings.
   
   Now you can open your browser and go to http://192.168.8.1 and see the dashboard where it will ask for SIMUNLOCK CODE or NETWORK UNLOCK Code
   
6. Calculate DATALOCK Code and NETWORK UNLOCK Code

Congrats you have unlocked your modem !!

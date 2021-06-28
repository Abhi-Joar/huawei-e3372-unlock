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
   
   Under root permission 
   *./balong-usbdload -p /dev/ttyUSB0 usblsafe_e3372h.bin* 
   will flash safeloader and the modem will enter into download mode after 10s-15s  led will start to blink  
   
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
   
   In this mode *12d1:1442* you will also have to flash the firmware again
   
   *./balong_flash -p /dev/ttyUSB0 E3372h-607_Update_22.200.05.00.00_universal.exe* 
   
   Bingo your downgrading and WebUI installation is complete. The modem will be reset and you can see the Wired connection is set up automatically in your Linux   Network settings.
   
   Now you can open your browser and go to http://192.168.8.1 and see the dashboard where it will ask for SIMUNLOCK CODE or NETWORK UNLOCK Code
   
### 5. Calculate DATALOCK Code and NETWORK UNLOCK Code
   After you have downgraded the firmware you are free to access the NVRAM from which we will extract data to calculate DATALOCK Code and the NETWORK CODE
   
   In the */etc/usb_modeswitch.conf* change the *DisableSwitching* variable to *1*
   
   Plugin the modem again and this time you will see modem in DataStorage mode *12d1:1f01* . Change the it to HuaweiAltMode by typing this command  
  
   *usb_modeswitch -v 12d1 -p 1f01 -W -X*
   
   This will switch the mode to serial *12d1:155e*  and *ls /dev/ttyUSB\** should show 3 ports *ttyUSB0 ttyUSB1 ttyUSB2*
   
   Install *minicom* or *atinout* to  send *AT* commands to modem to extract NVRAM data . For Datalock issue the following command 
   
   *echo "at^nvrdex=50502,0,128" | atinout - /dev/ttyUSB0 -* 
   
   It will output something like this
   
   *^NVRDEX: 50502,0,128,A9 E1 25 0E AB EE 1F 64 09 8B 1C 5B 96 E9 C0 A0 17 AE 71 0A E8 74 BF 8A 18 63 75 8B DA A3 39 C7 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 FF 51 96 A2 62 FD F5 20 C9 8A 7C AB 69 60 C9 83 6F AD 27 37 9B 75 A8 92 D1 C1 0E 1C 9E 7B 86 95 01 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00*
   
   Similarly  issue the following command for Network code
   
   *echo "at^nvrdex=50503,0,128" | atinout - /dev/ttyUSB0 -* 
   
   It will output something like this
   
   *^NVRDEX: 50503,0,128,A9 E1 25 0E AB EE 1F 64 09 8B 1C 5B 96 E9 C0 A0 17 AE 71 0A E8 74 BF 8A 18 63 75 8B DA A3 39 C7 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 FF 51 96 A2 62 FD F5 20 C9 8A 7C AB 69 60 C9 83 6F AD 27 37 9B 75 A8 92 D1 C1 0E 1C 9E 7B 86 95 01 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00*
   
   These two outputs will be used to calculate Datalock codes and Network unlock code
   
   After you get the NETWORK Code you can input in http://192.168.8.1 and Unlock your modem 
   

#### Congrats you have unlocked your modem ####
#### Send email to crackiitweb@gmail.com to get more help #### 
#### THIS IS A FREE SERVICE HOWEVER IF IT IS HELPFUL YOU CAN DONATE paypal.me/jordertech  ####


# huawei-e3372-unlock

Simple Process To Unlock Huawei E3372h in Linux and Windows 7
This method is for Auth V4 Modems only.  Check your modem whether it is v4

Firmware version applied on 22.200.15.0.210 

## Linux method
### 1. Boot Pin Short
  Install *usb_modeswitch* and *lsusb* if it is not already installed.
  
  Open */etc/usb_modeswitch.conf* under *root* and change *DisableSwitching* variable to *0*
  
  You have to open up your modem and short one of the pin to ground and insert it into usb while holding it in this shorted position. You can short the pin as shown in the figure with a compass or tweezers or small scissor. Hold it in this position for a minute. The led should be off , it should not blink , if it blinks then you have not shorted properly and you have to do it again.
  
  Once you shorted your modem properly you will enter into boot mode , check with *lsusb* and *dmesg* the device configuration should be *12d1:1443*  and in *dmesg* it should show as *USB CON HUAWEI* 

2. Flash Safeloader  

3. Flash Downgrade Firmware

4. Flash WebUI

6. Calculate DATALOCK Code and NETWORK UNLOCK Code

Congrats you have unlocked your modem !!

# Updating Modem Firmware - Thingy:91

This tutorial will show you how to upgrade the modem firmware on your Nordic Semiconductor Thingy:91. We strongly recommend keeping the modem firmware up to date in order to get new patches and protect your modem against known vulnerabilities. 

## 1. Download the firmware from nordicsemi.com

Go to https://www.nordicsemi.com/Software-and-tools/Prototyping-platforms/Nordic-Thingy-91/Download#infotabs and download the newest "Precompiled application and modem firmware". 

![Download FW](https://github.com/TelenorStartIoT/tutorials/blob/master/08-Thingy-modem-fw-upgrade/assets/01_Firmware_Thingy.PNG)


Unzip the downloaded folder. You should now have a regular folder with the following contents. Do not unzip the mfwnrf9160xx folder. 

![Unzip folder](https://github.com/TelenorStartIoT/tutorials/blob/master/08-Thingy-modem-fw-upgrade/assets/02_Firmware_Thingy.PNG)


## 2. Flash your Thingy:91 with the downloaded firmware

Open the Programmer app in nRF Connect. Connect your Thingy in MCU Boot mode by connecting it to the USB and press the middle button while turning the power on. Then scroll down the right-hand menu and press **Update modem** in the bottom of the menu. Then choose the mfwnrf9160xx.zip folder as shown below and press open. Then write to the Thingy and you are good to go.

![Open in nRF Connect](https://github.com/TelenorStartIoT/tutorials/blob/master/08-Thingy-modem-fw-upgrade/assets/03_Firmware_Thingy.PNG)

![Chose folder](https://github.com/TelenorStartIoT/tutorials/blob/master/08-Thingy-modem-fw-upgrade/assets/04_Firmware_Thingy.PNG)

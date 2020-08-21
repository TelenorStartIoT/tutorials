# Flash your Thingy:91 with new code (pre-compiled .hex file)

This is a continuation of tutorial "05-thingy-get-started", and we strongly reccomend going throug that before starting this tutorial. 

To flash your Thingy you need to install the "programmer" in nRF Connect application. 
Once installed, open the programmer. At the right hand side you find a panel with all the necessary functionality for flashing your .hex file.

1. Start by adding the file into the programmer application. You press the “Add HEX file” and select the hex file you would like to flash to your Thingy 91.


![](https://github.com/TelenorStartIoT/tutorials/blob/master/06-thingy-flash/assets/2.1-add-hex.png)


![](https://github.com/TelenorStartIoT/tutorials/blob/master/06-thingy-flash/assets/2.2-hex-uploaded.PNG)


2. Then connect your Thingy 91 to the computer through the USB port. To flash, your
Thingy 91 has to be in MCU boot mode. You turn this mode on by holding in the top
button while turning the Thingy 91 on.

3.  Select your device in the top left drop-down menu.


![](https://github.com/TelenorStartIoT/tutorials/blob/master/06-thingy-flash/assets/2.3-choose-device.PNG)


4. Now all you have to do is press the “Write” button on the right hand side panel. A
pop-up window appears and you have to press Write in this as well.


![](https://github.com/TelenorStartIoT/tutorials/blob/master/06-thingy-flash/assets/2.4-write-file.PNG)


5. The file transfer should start and complete without further steps.


![](https://github.com/TelenorStartIoT/tutorials/blob/master/06-thingy-flash/assets/2.5-writing.PNG)

![](https://github.com/TelenorStartIoT/tutorials/blob/master/06-thingy-flash/assets/2.6-download-complete.PNG)


6. Your Thingy 91 should now be sending to the “Thing” you provisioned in MIC. It might take some seconds, and if it does not work right away we recommend you turn the Thingy 91 off and on again.

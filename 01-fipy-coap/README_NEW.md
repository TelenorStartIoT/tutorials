# FiPy Dev-Kit - CoAP

> Works with NB-IoT or LTE-M

This tutorial gives brief instructions on how to get started with the FiPy dev-kit. This tutorial will send JSON formatted data in a CoAP packet over the LTE-M (Cat M1) or NB-IoT (NB1) network.

You will learn how to:

   * download and install the VSCode editor and the Pymakr plugin
   * assemble the dev-kit and connect it to the VSCode editor
   * create a Telenor Managed IoT Cloud (MIC) platform account
   * register your dev-kit in MIC and create payload transformations
   * flash the modem firmware on the FiPy (to support either LTE-M or NB-IoT)
   * program the dev-kit and send data to MIC over Telenor's excellent 4G LTE-M or NB-IoT network
   * view your data in MIC

You can also find a lot of info related to the FiPy on Pycom's own documentation site: https://docs.pycom.io/gettingstarted/


## 1. Sign Up For a MIC Platform Account

If you already have a MIC account you can start directly at step 2. 

You can **Sign-up** for a MIC account here: https://demonorway.mic.telenorconnexion.com

![Login](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/07-login-mic.png)

Click on the "Sign Up" button in the upper right corner and follow the instructions in order to sign up. It is important that you fill out at least the **six first fields** in addition to **company**. This is a prerequisite for being verified as a user. 

**Note:** You should be aware that the signup is two-phased, and after you have confirmed your email address it may take some time before your user account is activated. This is a manual verification process and may take up to 24 hours outside normal business hours. Once your account is ready for use you will receive an email stating that your account has been activated. 
You can however continue.


## 2. Download the software you need

To connect and program the FiPy dev-kit you need to download and install:

   * Visual Studio Code editor (VSCode editor)
   * Pymakr plugin in VSCode editor
   
If you already have these programs on your computer you can jump straight to step 3. 

### 2.1 Visual Studio Code Editor (VSCode editor)

VSCode editor is a program that allows you to write your code and easily upload (flash) it to your dev-kit. You can download the VSCode editor for Windows, Linux and MacOS here: https://code.visualstudio.com/download. Choose the right option for your operating system and download and install the program. 

![Download VSCode](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/00-download-vscode.png)

### 2.2 Pymakr Plugin for VSCode

When you have successfully installed VSCode editor you need to install the plugin 'Pymakr'. This plugin enables you to connect the FiPy dev-kit to the VSCode editor over USB. 

To install 'pymakr' plugin:
   1. Open VSCode
   2. Click on extension icon (as marked in picture below) 
   3. Find the extention by typing 'pymakr' in search bar. 
   4. Click on install

![VSCode Pymakr plugin](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/01-instal_pymakr_plugin.png)

When both the VSCode editor and the Pymakr plugin have been successfully installed you are ready to connect and start programming your FiPy. The next chapter will show you how to assemble your FiPy dev-kit. 


## 3. Assemble the FiPy Dev-Kit

In this chapter you will learn how to assemble and connect the FiPy dev-kit to the VSCode editor. You will also communicate with the dev-kit in order to check the firmware version.

You will assemble these components to make your dev-kit operable:
   * FiPy
   * LTE antenna
   * SIM card
   * Expansion board


### 3.1 Attach the Antenna and Insert the SIM Card to the FiPy

**Important**: You should always mount the 4G antenna to the connector on the **underside of the FiPy** (where the pins are). The result of not connecting the antenna right could be that you harm the modem when it is connected.

The connector for the antenna is located on the right-hand side once the FiPy is turned with its pins upwards. It might be a little hard to connect the antenna, but just press it on like a snap-button. 

![LTE antenna](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/02-Antenna.PNG)


The SIM card goes into the slot on the *bottom-left* side of the FiPy.
Insert the *Nano SIM* card. 

![Insert SIM](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/03SIM.png)

  **Notice the cut-corner of the SIM card to get the correct orientation. If you do not have the SIM inserted it will be impossible to connect to a cellular network.**


### 3.2 Mount FiPy on the Expansion Board

You can now mount the FiPy module to the provided Expansion Board. 

The USB connector on the expansion board must be on the same side as the reset button on the FiPy module (see picture below).

You should also check that all the PINs on the FiPy are matching the open connectors on the expansion board (i.e. that it is aligned correctly) before you push the FiPy module into the expansion board.

The image below shows you what it should look like. 

![Expansion board 3.0](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/04ExpansionBoard.png)

You now have a device that can communicate with the VSCode editor.


### 3.3 Connect the FiPy to the VSCode Editor

The next step is to connect your device to your computer and the VSCode editor. 

Open VSCode on your computer and connect your device using a USB cable. The Pymakr plugin will automatically detect your dev-kit and connect VSCode to it. 

![FiPy connected to VSCode and Pymakr](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/05-fipy-connected.png)

If your dev-kit is not automatically detected you can do it manually. Either by pressing **"Pycom Console"** to the left on the blue line or via the menu **"All Commands" > "Pymakr > Connect"**. 

![Connect FiPy manually](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/Troubleshoot-connect-FiPy.png)


If you don't see the Pycom Console window you can open it though the window menu **"Window" > "New Terminal"** and switch to **"Pycom Console"** in the drop-down menu.


### 3.4 Find Board Firmware Version

If you select the **"All commands"** button (located at the bottom bar) and then select "Get board version" in the VSCode Pymakr menu you should able to see the version number of the underlying Pycom Firmware. 

![FiPy board version](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/06-pymakr-board-version.png)


### 3.5 Upgrade the Pycom Firmware 

In most cases it is ok to use the firmware that is pre-installed on your FiPy but we recommend you upgrade it to the newest version to make sure everything is up to date. 

New versions and explanations of how to do the upgrade are made available on Pycom's own site here: https://docs.pycom.io/gettingstarted/installation/firmwaretool.html

You are now ready to start creating your use case and register your device as a 'Thing' in the IoT platform Telenor Managed IoT Cloud.
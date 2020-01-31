# FiPy Dev-Kit - MQTT/TLS 1.2

> Works with NB-IoT and LTE-M


This tutorial gives you step by step instructions on how to get started with your FiPy dev-kit. You will learn how to program your dev-kit and send data with the MQTT protocol using TLS 1.2 encryption directly to Telenor Managed IoT Cloud (MIC) over the LTE-M (Cat M1) or NB-IoT (NB1) network.

You will learn how to:

   1. Sign up for a MIC platform account
   2. Download the software you need
   3. Assemble the dev-kit
   4. Register your FyPy dev-kit as a thing in MIC
   5. Upgrade device firmware if needed
   6. Program your FyPy dev-kit
   7. Visualise your data in MIC and tailor your dashboard

You can also find a lot of info related to the FiPy on Pycom's own documentation site: https://docs.pycom.io/gettingstarted/

## 1 Sign Up For a MIC Platform Account

If you already have a MIC account you can start directly at step 2. 

You can **Sign-up** for a MIC account here: https://demonorway.mic.telenorconnexion.com

![Login](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/07-login-mic.png)

Click on the "Sign Up" button in the upper right corner and follow the instructions in order to sign up. It is important that you fill out at least the **six first fields** in addition to **company**. This is a prerequisite for being verified as a user. 

**Note:** You should be aware that the signup is two-phased, and after you have confirmed your email address it may take some time before your user account is activated. This is a manual verification process and may take up to 24 hours outside normal business hours. Once your account is ready for use you will receive an email stating that your account has been activated. 
You can however continue


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

To install 'pymakr' plugin 
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

The USB connector on the expansion board must be on the same side as the reset button on the FiPy module. (see picture below) 

You should also check that all the PINs on the FiPy are matching the open connectors on the expansion board (i.e. that it is aligned correctly) before you push the FiPy module into the expansion board.

The image below shows you what it should look like. 

![Expansion board 3.0](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/04ExpansionBoard.png)

You now have a device that can communicate with the VSCode editor.


### 3.3 Connect the FiPy to the VSCode Editor

The next step is to connect your device to your computer and the VSCode editor. 

Open VSCode on your computer and connect your device using a USB cable. The Pymakr plugin will automatically detect your dev-kit and connect VSCode to it. 

![FiPy connected to VSCode and Pymakr](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/05-fipy-connected.png)

If your dev-kit is not automatically detected you can do it manually. Either by pressing "Pycom Console" to the left on the blue line or via the menu "all commands" > "Pymakr>Connect". 

![Connect FiPy manually](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/Troubleshoot-connect-FiPy.png)


If you don't see the Pycom Console window you can open it though the window menu *"Window" > "New Terminal"* and switch to *"Pycom Console"* in the drop-down menu.


### 3.4 Find board firmware version

If you select the **"All commands"** button (located at the bottom bar) and then select "Get board version" in the VSCode Pymakr menu you should able to see the version number of the underlying Pycom Firmware. 

![FiPy board version](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/06-pymakr-board-version.png)


### 3.5 Upgrade the Pycom Firmware 

In most cases it is ok to use the firmware that is pre-installed on your FiPy but we recommend you upgrade it to the newest version to make sure everything is up to date. 

New versions and explenations of how to do the upgrade are made available on Pycom's own site here: https://docs.pycom.io/gettingstarted/installation/firmwaretool.html

You are now ready to start creating your use case and register your device as a 'thing' in the IoT platform Telenor Managed IoT Cloud.


## 4. Register Your FiPy Dev-Kit in Telenor Managed IoT Cloud as an MQTT Thing

You are now finished using VSCode for a little while and in this next section you just need to log in to your MIC account in an internet browser. 

In this chapter you will learn how to use the Telenor Managed IoT Cloud (MIC) platform. 

These are the steps chapter 4 will take you through:
4.1 Create a "Thing Type" for your dev-kit
4.2 Add a "Thing" representing your dev-kit

When you have completed this chapter you are ready to start programming your dev-kit and visualise the data in MIC. 

### 4.1 Create a "Thing Type" for your dev-kit

Once your MIC account is activated, you will be able to login to your MIC account with your user id and password.

When you log in for the first time you can start by creating a new "Thing Type". 
   * A "Thing Type" is a method of organising multiple "Things" that report on the same data resources. 
   * A "Thing" must always belong to a "Thing Type", this is why we have to make this before registering a "Thing".


To add a *new* **"Thing Type"** click on the **"+NEW THING TYPE"** button and give it a name and a description. 
* Assign it to your domain in the drop-down menu.  

Note: For things that will communicate with the **MQTT protocol** it is **not necessary to add uplink or downlink transformations** since the thing will send data following the MIC MQTT shadow update format. This format is directly understood by MIC. 

![Create Thing Type](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/08-thing-type.png)

For use cases where other protocols than MQTT is used, the uplink and downlink transforms will decide the payload handeling of all the *Things* within a *Thing Type*. This however is not relevant for this tutorial. 


### 4.2 Add a "Thing" Representing Your Dev-Kit

Once you have established a *"Thing Type"* you can add your dev-kit as a *"Thing"*.

Your *"Thing Types"* will be visible to you in the panel on the left hand side of the window. When creating a new *"Thing"* it will automatically belong to the selected *"Thing Type"*.

To add a new Thing, click on the **"+ THINGS"** button.

![Add new Thing to Thing Type](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/09-new-thing.png)

A pop-up window will appear. 
   * De-select the **"Create batch"** slider in the pop-up window.
   * You must then add a **"Thing Name"**, a **"Description"** and select your **"Domain"** from the drop-down menu.
   * Choose **"MQTT"** as the protocol for your Thing.

The image shows an example of what it should look like.

![Configure new Thing in Thing Type](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/10-thing.png)

The things list will now show the Thing that you just created.

![New Thing in MIC](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/12-thing-list.png)

You are now finished registering your thing in MIC, and you can start working with the dev-kit again. 


## 5. Flash FiPy for NB-IoT or Update the LTE-M Firmware

**NB:** This step is only necessary if you are going to use the NB-IoT network. If you plan to use the LTE-M network this step is voluntary.

The modem is the part of your dev-kit that enables it to communicate with different networks. Currently the FiPy comes with two separate modem firmware images. One image is for NB-IoT (NB1) and the other image is for LTE-M (Cat M1). Unfortunately only one of the modem images can be used at a time. This means that you need to make sure the right modem firmware image is flashed to the FiPy according to which network you want to use. In the future we hope that this will change and that it will be possible to switch between the two network types in your own program for the FiPy without having to flash the modem. However, for the time being this needs to be done manually.

The FiPy is configured with the LTE-M (Cat M1) modem firmware as default from the factory, so if you want to use the LTE-M network you do not have to flash the modem with new firmware and you can skip this chapter.

It could however be a good idea to upgrade the LTE-M firmware when new versions are released. If you intend to use NB-IoT network you must flash the modem firmware, as the factory image of the FiPy modem is configured for LTE-M (Cat M1).
Detailed instructions on how to update the LTE-M firmware or switch to NB-IoT firmware can be found here: https://docs.pycom.io/tutorials/lte/firmware.html

**Note:** We recommend using an SD card in this process, but feel free to follow the method most convenient to you. 

Once you have the right modem firmware on your dev-kit you can start programming. 

## 6. Program the FiPy Using MQTT 

In this chapter you will learn how to program the FiPy dev-kit to send data using MQTT directly to MIC. The chapter will teach you how to use the provided example code to connect the FiPy to the LTE-M or NB-IoT networks and publish dummy messages directly to Telenor Managed IoT Cloud.

The chapter will take you through these steps:
   1. Download and save the example code from GitHUB 
   2. Download and save certificates and keys for your thing from MIC
   3. Open the code in VSCode
   4. Add the ThingID to your program's MQTT configuration
   5. Modify the program's network configuration
   6. Upload the program to your FiPy
   7. Check the output of the program

### 6.1 Download Example Code

You can download the example code from here: https://github.com/TelenorStartIoT/fipy-dev-kit-mqtt

You should choose the "Download ZIP" option in the "Clone or Download" button pop-up. This will download a ZIP archive with the example code.

![Download ZIP](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/14-download-zip.png)

Unzip the example code. How to do this varies depending on your computer system. Most systems will unzip it if you double click on the zip file.

Give the folder a *"name you can remember"* and save it to someplace you can find back to it. 


### 6.2 Download the Certificates and Keys From MIC

Download the certificates and keys for your Thing from MIC as the image shows (you will get a ZIP file). 

![Download certificates and keys](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/17-download-certs.png)

Unzip the downloaded folder and add the three files (cert.pem, privkey.pem and pubkey.pem) to the "cert" folder within the GitHub code folder from the previous step. 

You should now have a folder with a *"name you can remember"* containing this:

![Folder with project code](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/folder-with-a-name-you-can-remember.png)

Next step is to open the folder in the VSCode editor.

### 6.3 Open the Code In VSCode

The folder you created in the previous step contains everything you need for your project code. Once you have opened the folder in the VSCode editor, you can edit and change the code. To save the changes you make, press (ctrl+s).  

First, open VSCode and then open your folder using the "File > Open Folder" ("File > Open" on MacOS) option.

![Open project in VSCode](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/15-open-project.png)

You can now see your folder in the panel on the left hand side of the VSCode window. The main code is in the main.py file, which calls upon configurations and certificates that are located other places in the folder. 

![Open project in VSCode](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/16-open-project.png)

Check that the cert-folder contains the three certificates and keys you downloaded from MIC in addittion to the root certificate. If it doesn't you must go back to step two and make sure the certificates are saved in the cert folder. 

### 6.4 Modify the Program's MQTT Configuration

You are now going to write/change some parameters in the code to make it spesific to your "Thing" (dev-kit). 
Make sure you save your changes by pressing (ctrl+s).

The first parameter you have to add to your code is a "Thing ID". You can find this in MIC.  
To see your Thing ID, click on the + (or III) sign in the list view and check the unchecked Thing ID box (see image). The Thing ID for your things will now appear in the *Things List*. 

![View Thing ID](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/18-view-thing-id.png)

Note your Thing ID (00000XXX) then open the main.py file in the VSCode and change the Thing ID variable at line 17 of the file. 

``` python
# Your Telenor Managed IoT Cloud (MIC) Thing configuration.
# Change this to your Thing name / ID:
THING_ID = '00000XXX'
```


### 6.5 Modify the Program's Network Configuration

Note: If you are using the LTE-M network, you do not have to change the network configurations

The program you downloaded is by default configured to use the LTE-M (Cat M1) network. If you open the [main.py](https://github.com/TelenorStartIoT/fipy-dev-kit-mqtt/blob/master/main.py) file you will se the following code at line 27-32:

``` python
# Create a new Telenor Start IoT object using the LTE-M network.
# Change the `network` parameter if you want to use the NB-IoT
# network like this: iot = StartIoT(network='nb-iot')
# You must flash the correct Sequans modem firmware before
# changing network protocol!
iot = StartIoT(network='lte-m')
```

As described in the code comments, if you are using the NB-IoT network you must change the `network` parameter to `nb-iot`:

``` python
iot = StartIoT(network='nb-iot')
```
**If you are using MQTT you do not have to change anything in this file and can go straight to uploading the program to your FiPy.** 

Note: Remember that if you want to use the NB-IoT network you will also have to flash the Sequans modem on the FiPy with the NB-IoT firmware as described in previous chapters.

### 6.6 Upload the Program to your FiPy

Connect the FiPy that is mounted on the expansion board to your computer (if not already). Make sure that the SIM and LTE antenna is connected. The Pymakr plugin in VSCode will automatically detect the dev-kit.

**Make sure you have saved your code after writing the modifications. You save by pressing (ctrl+s).**

To upload and run the program on your FiPy, simply click the "Upload" button located at the bottom bar in VSCode. This will first upload the code and the certificate and key, then it will reset your FiPy and run the uploaded code.

**Note:** With dev-kits it is very common to get errors while trying to do this. We have listed some tips for you to try if you get errors, and if none of these work, visit our [forum](https://startiot.telenor.com/forums/forum/end-devices/pycom-fipy/) for more possible solutions. 

Tips and tricks if you get errors: 
   * Check that your certificate files are located in the cert folder.
   * Put your device in safe boot mode right before uploading the code. You can put your FiPy in safe boot mode by holding down the "Safe Boot" button on the expansion board while pressing the reset button (the only buttonon the top of the FiPy). The light should flash orange. Try again to press upload. Try this a couple of times if it doesn't work the first time. Sometimes the fith or sixth time's the charm. 
   * Disconnect and re-connect the dev-kit in VSCode. Try again with the previous tip. 
   * Take the USB out of the computer and put it back in. 
   * Try again (and again...)
   * Check the Start IoT forum for more advanced troubleshooting. 


### 6.7 Check the Output From the Program

If everything succeeds you should see output from the program in the Pycom Console window in VSCode (bottom part of the window). The image shows what it could look like (the output from the program might be different).

![Program output](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/18-program-output.png)

If you have trouble with connecting to the network, revisit the tips in the previous section or visit our [forum](https://startiot.telenor.com/forums/forum/end-devices/pycom-fipy/) where many solutions are posted. Here you can ask questions or learn from other users' questions and answers.  

## 7 Visualise your data in MIC

Now your dev-kit is sending dummy data on temperature and humidity over the chosen cellular network. The next step is to view and organise your data in MIC. 

### 7.1 View and edit your Dashboard

If you click on the name of your *Thing* in the list you will open the dashboard for your Thing. This dashboard will be mainly empty until the first payload from your dev-kit arrives. The dashboard is configurable and you can add widgets that represents values sent from your dev-kit. The diferent values are called resources. For example: Temperature can be a resource. 

To edit your dashboard you have to press the *move* button in the top right corner. This will enable edit mode where you can move things around and create new widgets. 

![Edit mode](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/22-Edit-dashboard.PNG)

Once you are in edit mode you can create new widgets and tailor the dashboard to your use-case. **Remember to press save** when you have done new changes, otherwise the edits will be lost. 

![Create and save](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/23-Add-Widget-And_Save.PNG)


### 7.2 Create Widgets

You should now be able to view your data in the MIC dashboard. Try to add a time series widget by clicking the “+WIDGET” button in MIC and set the type to be "Time Series". Images below shows some sample screenshots on how to do it.

![Timeseries widget](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/19-timeseries-widget.png)

The image below shows an example of how a very simple dashboard can look like. The MIC platform have many advanced options for widgets so your dashboard can be tailored very spesifically to your use case and data.

Note: When sending MQTT data to MIC it will be possible to view the separate values in the MQTT publish packet in MIC widgets. In cases where other protocols are used the resources have to be defined in the uplink transform before they are available to the widgets. 

![Dashboard](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/13-sample-dashboard.png)

### 6.9 Happy Hacking!

This concludes the "Get Started With the FiPy Dev-Kit (UDP over NB-IoT or LTE-M)" tutorial. Your next step could be to connect the supplied DHT11 sensor to the FiPy and to modify the "dummy" payload string with values from the DHT11 sensor.

A god starting point would be to use the library code supplied here:

https://github.com/JurassicPork/DHT_PyCom/tree/pulses_get

Happy hacking!

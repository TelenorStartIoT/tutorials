# FiPy Dev-Kit - MQTT/TLS 1.2

> Works with NB-IoT and LTE-M


This tutorial gives you step by step instructions on how to get started with your FiPy dev-kit. You will learn how to program your dev-kit and send data with the MQTT protocol using TLS 1.2 encryption directly to Telenor Managed IoT Cloud (MIC) over the LTE-M (Cat M1) or NB-IoT (NB1) network.

You will learn how to:

   1. Download the software you need
   2. Assemble the dev-kit
   3. Create an account in Telenor Managed IoT Cloud (MIC)
   4. Register your device (dev-kit) as a thing in MIC
   5. Update device firmware if needed
   6. Program your dev-kit
   7. Send data to MIC using MQTT over Telenor's excellent 4G LTE-M or NB-IoT network
   8. View your data in MIC and tailor your dashboard

You can also find a lot of info related to the FiPy on Pycom's own documentation site: https://docs.pycom.io/gettingstarted/

## 1. Download the software you need

To connect and program the FiPy dev-kit you need to download and install:

   * Visual Studio Code editor (VSCode editor)
   * Pymakr plugin in VSCode editor

### 1.1 Visual Studio Code Editor (VSCode editor)

VSCode editor is a program that allows you to write your code and easily upload (flash) it to your dev-kit. You can download the VSCode editor for Windows, Linux and MacOS here: https://code.visualstudio.com/download. Choose the right option for your operating system and download and install the program. 

![Download VSCode](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/00-download-vscode.png)

### 1.2 Pymakr Plugin for VSCode

When you have successfully installed VSCode editor you need to install the plugin 'Pymakr'. This plugin enables you to connect the FiPy dev-kit to the VSCode editor over USB. 

To install 'pymakr' plugin 
   1. Open VSCode
   2. Click on extension icon (as marked in picture below) 
   3. Find the extention by typing 'pymakr' in search bar. 
   4. Click on install

![VSCode Pymakr plugin](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/01-instal_pymakr_plugin.png)

When both the VSCode editor and the Pymakr plugin have been successfully installed you are ready to connect and start programming your FiPy. The next chapter will show you how to assemble your FiPy dev-kit. 


## 2. Assemble the FiPy Dev-Kit

In this chapter you will learn how to assemble and connect the FiPy dev-kit to the VSCode editor. You will also communicate with the dev-kit in order to check the firmware version.

You will assemble these components to make your dev-kit operable:
   * FiPy
   * LTE antenna
   * SIM card
   * Expansion board

### 2.1 Attach the Antenna and Insert the SIM Card

The LTE antenna and the SIM card are mounted directly to the FiPy.

**Important**: You should always mount the 4G antenna to the connector on the bottom side of the FiPy. The result of **not connecting** the antenna right could be that you harm the modem when it is connected.

There is a slot to attach the antenna on the *bottom-right* side of the FiPy. It might be a little hard, but just press it on like a snap button. 

![LTE antenna](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/02-lte-ant-fipy_new.png)


The SIM card goes into the slot on the *bottom-left* side of the FiPy.
Insert the *Nano SIM* card. 

![Insert SIM](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/03-sim-fipy-new1.png)

  **Notice the cut-corner of the SIM card to get the correct orientation. If you do not have the SIM inserted it will be impossible to connect to a cellular network.**

### 2.2 Mount FiPy on the Expansion Board

You can now mount the FiPy module to the provided Expansion Board. 

The USB connector on the expansion board must be on the same side as the reset button on the FiPy module. (see picture below) 

You should also check that all the PINs on the FiPy are matching the open connectors on the expansion board (i.e. that it is aligned correctly) before you push the FiPy module into the expansion board.

The image below shows you what it should look like. 

![Expansion board 3.0](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/04-expansion-board-3-new-fipy.png)

You now have a device that can communicate with the VSCode editor.


### 2.3 Connect the FiPy to the VSCode Editor

The next step is to connect your device to your computer and the VSCode editor. 

Open VSCode on your computer and connect your device using a USB cable. The Pymakr plugin will automatically detect your dev-kit and connect VSCode to it.

![FiPy connected to VSCode and Pymakr](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/05-fipy-connected.png)

If you don't see the Pycom Console window you can open it though the window menu **"Window" > "New Terminal" and switch to "Pycom Console" in the drop-down menu.**

### 2.4 Find board firmware version

If you select the **"All commands"** button (located at the bottom bar) and then select "Get board version" in the VSCode Pymakr menu you should able to see the version number of the underlying Pycom Firmware. 

![FiPy board version](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/06-pymakr-board-version.png)

### 2.5 Upgrade the Pycom Firmware 

In most cases it is ok to use the firmware that is pre-installed on your FiPy but we recommend you upgrade it to the newest version to make sure everything is up to date. 

New versions and explenations of how to do the upgrade are made available on Pycom's own site here: https://docs.pycom.io/gettingstarted/installation/firmwaretool.html

## 3. Register Your FiPy Dev-Kit in Telenor Managed IoT Cloud as an MQTT Thing

In this chapter you will learn 
* How to register your dev-kit in Telenor Managed IoT Cloud (MIC). 
* How to add a new MQTT "Thing" in MIC. 
* How to create a dashboard to display your data. When sending MQTT data to MIC it will be possible to view the separate values in the MQTT publish packet in MIC widgets. Widget types in MIC ranges from simple textual widgets to graphical representations of your data.

### 3.1 Sign Up For a MIC Platform Account

**Sign-up** for a MIC account in order to register your dev-kit. You can do that here: https://demonorway.mic.telenorconnexion.com

![Login](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/07-login-mic.png)

Click on the "Sign Up" button in the upper right corner and follow the instructions in order to sign up. 

Note: You should be aware that the signup are two phased sign up. 
* In Phase one, verify your email. We will send a link to the email you register and you will have to use the link to verify your email address. 
* In phase two, we will manually register your private MIC domain and activate your account. You will then receive a second email stating that your account has been activated. **Because of this procedure it may take up to 24 hours before your account is ready to be used.**

### 3.2 Add a New Thing Type

Once we approved your MIC account, you will be able to login to your MIC account with your user id and password.

When logged in create a new "Thing Type" for your dev-kit.
-  A things is a virtual representation of your IoT device.
-  A "Thing Type" is a way to organize multiple "Things" that share similarities.

To add a *new* **"Thing Type"** click on the **"+NEW THING TYPE"** button and give it a name and a description. 
* Assign it to the domain that your user was added to. 

Note: For things that are of type MQTT it is not necessary to add uplink or downlink transformations since the thing will send data following the MIC MQTT shadow update format. This format is directly understood by MIC.

You should keep in mind that this may increase the payload size for each data packet that your IoT device sends.

![Create Thing Type](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/08-thing-type.png)

### 3.3 Add a Thing Representing Your Dev-Kit

The *"Thing Type" and "Thing"* together is a representation of your dev-kit in MIC. 
   
It is possible to have more than one thing in a Thing Type and this will make the Things in the Thing Type behave in the same manner with respect to how payloads from the Things are handled. The handling of the payload is described in your uplink transformation. When using the MQTT protocol we will however not make use of the uplink transformation.

To add a new Thing, click on the **"+ THINGS"** button.

![Add new Thing to Thing Type](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/09-new-thing.png)

A pop-up window will appear. De-select the **"Create batch"** slider in the pop-up window.
- You must then add a **"Thing Name"**, a **"Description"**, select your **"Domain"**.
- Choose **"MQTT"** as the protocol for your Thing.

The image shows an example of what it should look like.

![Configure new Thing in Thing Type](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/10-thing.png)

### 3.4 See Your Newly Created Thing

You can access your Thing if you click the **"Thing List"** button and then **"ADD THING LIST WIDGET"**. This will add a widget to the Thing Type view with a list of Things.

![New Thing in MIC](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/11-thing-list-widget.png)

The Thing List widget will show our single Thing that we just created.

![New Thing in MIC](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/12-thing-list.png)

### 3.5 Example Dashboard

If you click on the **"Thing name"** in the list you will create a dashboard for your Thing. The dashboard will be mainly empty until the first payload for your Thing arrives. The dashboard is configurable and you can add widgets that represents values sent from your dev-kit (called resources). The image shows a very simple dashboard for what it could look like. 
It is possible to add more advanced widgets.

![Dashboard](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/13-sample-dashboard.png)

### 3.6 Start Programming!

It is now time to start programming the dev-kit. In the next chapters we will show you how.

## 4. Flash FiPy for NB-IoT or Update the LTE-M Firmware

Currently the FiPy comes with two separate modem firmware images. One image is for NB-IoT (NB1) and the other image is for LTE-M (Cat M1). Unfortunately only one of the modem images can be used at a time but we hope that this will change in the future, and that it will be possible to switch between the two network types in your own program for the FiPy without having to flash the modem.

From the factory the FiPy is configured with the LTE-M (Cat M1) modem firmware, so if you want to use the LTE-M network you do not have to flash the modem with new firmware and you can skip this chapter.

It could however be a good idea to upgrade the LTE-M firmware when new versions are released. Detailed instructions on how to update the LTE-M firmware or switch to NB-IoT firmware can be found here: https://docs.pycom.io/tutorials/lte/firmware.html

If you intend to use NB-IoT you must flash the modem firmware if you haven't done so already, as the factory image of the FiPy modem is configured for LTE-M (Cat M1).

## 5. Program the FiPy Using MQTT (LTE-M or NB-IoT)

In this chapter you will learn how to program the FiPy dev-kit to send data using MQTT directly to MIC. The chapter will guide you through how to use the provided example code to connect the FiPy to the LTE-M or NB-IoT networks and publish dummy messages directly to Telenor Managed IoT Cloud. Be aware that the FiPy Sequans modem firmware can only support either LTE-M (Cat M1) or NB-IoT. If you want to experiment with the NB-IoT network you will have to flash the Sequans modem on the FiPy with the NB-IoT firmware first as described in the previous chapter.

### 5.1 Download Example Code

You can download the example code from here: https://github.com/TelenorStartIoT/fipy-dev-kit-mqtt

You should choose the "Download ZIP" option in the "Clone or Download" button pop-up. This will download a ZIP archive with the example code.

![Download ZIP](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/14-download-zip.png)

### 5.2 Unzip the Example Code and Open It In VSCode

Unzip the example code. How to do this varies depending on your computer system. Most systems will unzip it if you double click on the zip file.

Open the folder using the "File > Open Folder" ("File > Open" on MacOS) option in VSCode.

![Open project in VSCode](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/15-open-project.png)

![Open project in VSCode](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/16-open-project.png)

### 5.3 Download the Certificates and Keys From MIC

Download the certificates and keys for your Thing as the image shows (you will get a ZIP file). Unzip the downloaded file and and add the content to the "cert" folder in the code project (remove old certificates and keys from this folder if present).

![Download certificates and keys](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/17-download-certs.png)

### 5.4 Modify the Program's MQTT Configuration

You will have to change Thing ID configuration variable at the top of the [main.py](https://github.com/TelenorStartIoT/fipy-dev-kit-mqtt/blob/master/main.py) file of the downloaded project.

To see your Thing ID, click on the + sign in the list view and check the unchecked Thing ID box (see image).

![View Thing ID](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/18-view-thing-id.png)

Note your Thing ID in the list view and add it to the code:

``` python
# Your Telenor Managed IoT Cloud (MIC) Thing configuration.
# Change this to your Thing name / ID:
THING_ID = '00001965'
```

### 5.5 Modify the Program's Network Configuration

The program you downloaded is by default configured to use the LTE-M (Cat M1) network. If you open the [main.py](https://github.com/TelenorStartIoT/fipy-dev-kit-mqtt/blob/master/main.py) file you will se the following code at the top:

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

Remember that if you want to use the NB-IoT network you will also have to flash the Sequans modem on the FiPy with the NB-IoT firmware as described in previous chapters.

### 5.6 Run the Program

Connect the FiPy that is mounted on the expansion board to your computer (if not already). Make sure that the SIM and LTE antenna is connected! The Pymakr plugin in VSCode will automatically detect the dev-kit.

To upload and run the program on your FiPy, simply click the "Upload" button located at the bottom bar. This will first upload the code, the certificate and key, then it will reset your FiPy and run the uploaded code.

### 5.7 Check the Output From the Program

If everything goes well you should see output from the program in the Pycom Console window in VSCode. The image shows what it could look like (the output from the program might be different).

![Program output](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/18-program-output.png)

### 5.8 Wrapping It All Up

You should now be able to view your data in the MIC dashboard. Try to add a time series widget by clicking the “+WIDGET” button in MIC and set the type to be "Time Series". Images below shows some sample screenshots on how to do it.

![Timeseries widget](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/19-timeseries-widget.png)

![Example dashboard](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/20-example-dashboard.png)

### 5.9 Happy Hacking!

This concludes the "Get Started With the FiPy Dev-Kit (UDP over NB-IoT or LTE-M)" tutorial. Your next step could be to connect the supplied DHT11 sensor to the FiPy and to modify the "dummy" payload string with values from the DHT11 sensor.

A god starting point would be to use the library code supplied here:

https://github.com/JurassicPork/DHT_PyCom/tree/pulses_get

Happy hacking!

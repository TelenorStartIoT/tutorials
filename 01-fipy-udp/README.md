# Get Started With the FiPy Dev-Kit (UDP over NB-IoT or LTE-M)

> Reading time: 60 minutes

This tutorial gives brief instructions on how to get started with the FiPy dev-kit. This tutorial will send JSON formatted data in an UDP packet over the LTE-M (Cat M1) or NB-IoT (NB1) network.

You will learn how to:

   * download and install the VSCode editor and the Pymakr plugin
   * assemble the dev-kit and connect it to the VSCode editor
   * create a Telenor Start IoT Managed IoT Cloud (MIC) platform account
   * register your dev-kit in MIC and create payload transformations
   * flash the modem firmware on the FiPy (to support either LTE-M or NB-IoT)
   * program the dev kit and send data to MIC over Telenor's excellent 4G LTE-M or NB-IoT network
   * view your data in MIC

You can also find a lot of info related to the FiPy on Pycom's own documentation site: https://docs.pycom.io/gettingstarted/

## Contents

  1. [4G LTE Cellular IoT Networks](#1-4g-lte-cellular-iot-networks)
     1. [Coverage](#coverage)
     2. [NB-IoT (NB1)](#nb-iot-nb1)
     3. [LTE-M (Cat M1/M1)](#lte-m-cat-m1m1)
  2. [APN: Getting Data Out of the Cellular Network to an IP Endpoint](#2-apn-getting-data-out-of-the-cellular-network-to-an-ip-endpoint)
     1. [Public APN](#public-apn)
     2. [Private APN](#private-apn) 
  3. [Cellular LPWAN Modems, Devices and Development Kits](#3-cellular-lpwan-modems-devices-and-development-kits)
     1. [LPWAN Cellular Modems](#lpwan-cellular-modems)
     2. [IoT Devices](#iot-devices)
     3. [LPWAN Development Kits](#lpwan-development-kits)
  4. [IoT Platforms: Telenor Managed IoT Cloud and IoT Gateway](#4-iot-platforms-telenor-managed-iot-cloud-and-iot-gateway)
     1. [Telenor Managed IoT Cloud (MIC)](#telenor-managed-iot-cloud-mic)

## 1. Download and Install the VSCode Editor and the Pymakr Plugin

This chapter will show you how to download and install the VSCode editor and the Pymakr plugin. The Pymakr plugin together with the VSCode editor is what you will use to connect to and program your FiPy dev-kit.

### Download the VSCode Editor

You can download the VSCode editor for Windows, Linux and MacOS here: https://code.visualstudio.com/download. Download and install the VSCode editor for your operating system.

![Download VSCode](./assets/00-download-vscode.png)

### Install the Pymakr Plugin for VSCode

When you have successfully installed the VSCode editor you need to enable VSCode to connect over USB to the FiPy dev-kit. For that, the VSCode editor needs the Pymakr plugin. You should follow the instructions given by Pycom in order to install the plugin: https://docs.pycom.io/pymakr/installation/vscode/

![VSCode Pymakr plugin](./assets/01-pymakr.png)

When both the VSCode editor and the Pymakr plugin have been successfully installed you are ready to connect and start programming your own firmware for the FiPy. The next chapter will show you how to connect your dev-kit to the VSCode editor.

## 2. Assemble the FiPy Dev-Kit

In this chapter you will learn how to assemble and connect the FiPy dev-kit to the VSCode editor. You will also communicate with the dev-kit in order to check the firmware version and retrieve the IMSI and IMEI numbers that you will need to register you dev-kit in the MIC platform. More on that in a later chapter, let us first assemble the dev-kit.

### Attach the Antenna

**Important**: You should always mount the 4G antenna to the connector on the bottom side of the FiPy. The result of not connecting the antenna could be that you harm the modem when it is connected.

![LTE antenna](./assets/02-lte-ant-fipy.png)

Attach the antenna on the bottom side of the FiPy.

You should insert your SIM card into the SIM card slot on the bottom side of the FiPy. Notice the cut-corner of the SIM card to get the correct orientation. If you do not have the SIM inserted it will be impossible to connect to the 4G network.

![Insert SIM](./assets/03-sim-fipy.png)

Insert the Nano SIM card.

### Mount FiPy on the Expansion Board

You should now mount the FiPy module to the provided Expansion Board. The USB connector on the expansion board must be in the same direction as the reset button on the FiPy module. The image shows you what it should look like. You should check that all the jumper switches on the Expansion board is present. The jumper switches are the small plastic jumpers located on the upper left side of the expansion board. Without all the jumpers in place it might be impossible to get contact with the FiPy. You should also check that all the PIN's on the FiPy are matching the open connectors on the expansion board (i.e. that it is aligned correctly) when you push the FiPy model into the expansion board.

![Expansion board 3.0](./assets/04-expansion-board-3-fipy.png)

Mount the FiPy on the Expansion board.

### Connect the FiPy to the VSCode Editor

You should now be able to connect to the FiPy dev-kit using a USB port on your computer. The Pymakr plugin will automatically detect your dev-kit and connect VSCode to it. You should note that the micro USB cable is not supplied with the dev-kit.

![FiPy connected to VSCode and Pymakr](./assets/05-fipy-connected.png)

If you don't see the Pycom Console window you can open it though the window menu "Window" > "New Terminal" and switch to "Pycom Console" in the drop-down.

If you select the "All commands" button (located at the bottom bar) and then select "Get board version" in the VSCode Pymakr menu you should see the version number of the underlying Pycom Firmware.

![FiPy board version](./assets/06-pymakr-board-version.png)

In the next chapter you will need the IMSI and IMEI numbers of the dev-kit. To get that, type the following in the REPL (after the >>>) and include carriage return (press enter) after each line. Make a note of the IMSI and IMEI numbers displayed (or just keep the VSCode editor open and available).

``` python
from network import LTE
lte = LTE()
lte.send_at_cmd("AT+CIMI")
lte.send_at_cmd("AT+CGSN")
```

![IMSI and IMEI](./assets/07-imsi-imei.png)

### Upgrade the Pycom Firmware

It should be OK to use the firmware that is pre-installed on your FiPy but you should upgrade it following the procedures here when new versions are made available: https://docs.pycom.io/gettingstarted/installation/firmwaretool.html

## 3. Register Your FiPy Dev-Kit in Telenor Managed IoT Cloud

In this chapter you will learn how to register your dev-kit to Telenor Managed IoT Cloud (MIC). Using the IMSI and IMEI information you will learn how to add a new "Thing" in MIC. You will also learn how to add a transformation of your payload and how to create a dashboard. The payload transformation makes it possible for you to unpack data packets and view the individual parts of the payload in different types of widgets in the dashboard. Widget types ranges from simple textual widgets to graphical representations of your data.

### Sign Up For a MIC Platform Account

You will have to register for a MIC account in order to register your dev-kit. You can do that here: https://demonorway.mic.telenorconnexion.com

![Login](./assets/08-login-mic.png)

Click on the "Sign Up" button in the upper right corner and follow the instructions in order to sign up. You should be aware that the signup is a two phased sign up. It therefore requires that you, in phase one, verify your email. We will send a link to the email you register and you will have to use the link to verify your email address. In phase two, we will manually register your private MIC domain and activate your account. You will then receive a second email stating that your account has been activated. Because of this procedure it may take up to 24 hours before your account is ready to be used.

### Add a New Thing Type

You will be able to login to your MIC account when it is ready for use. When logged in create a new "Thing Type" for your dev-kit. A "Thing Type" is a way to organize multiple "Things" that share similarities.

To add a new "Thing Type" click on the "+NEW THING TYPE" button and give it a name and a description. Assign it to the domain that your user was added to. Finally, in the "Uplink Transform" add the following code:

``` js
return JSON.parse(payload.toString('utf-8'));
```

![Create Thing Type](./assets/09-thing-type.png)

This code is just one simple example of what the uplink transform can look like. In this case it will transform JSON formatted payloads into a JavaScript object and return it. This will separate each property of the object into its separate "parts". For each "part" a resource in MIC will be created.

Do not worry about the details for now, this was just for your information. It is possible to create uplink transformations for payloads formatted in basically any format (hex, binary, text, JSON, etc.). The uplink transform is just a snippet of JavaScript code that MIC will use when doing transformations on your payload, and is generally used to unpack small payloads into understandable resources.

### Add a Thing Representing Your Dev-Kit

The "Thing Type" and "Thing" together is a representation of your dev-kit in MIC. It is possible to have more than one Thing in a Thing Type and this will make the Things in the Thing Type behave in the same manner with respect to how payloads from the Things are handled. The handling of the payload is described in your uplink transformation.

To add a new Thing, click on the "+ THINGS" button. De-select the "Create batch" slider in the pop-up window. You must then add a "Thing Name", a "Description", select your "Domain" and choose "Protocol" for your Thing. When you select "UDP" as protocol you will also have to add the IMSI and IMEI numbers of your dev-kit. The image shows an example of what it should look like.

![Add new Thing to Thing Type](./assets/10-thing.png)

### See Your Newly Created Thing

You can look at and access your Thing if you click the "List" tab. The image shows an example list of devices reflecting a single dev-kit Thing.

![New Thing in MIC](./assets/11-thing-list.png)

### Example Dashboard

If you click on the "Thing name" in the list you will create a dashboard for your Thing. The dashboard will be mainly empty until the first payload for your Thing arrives. The dashboard is configurable and you can add widgets that represents values sent from your dev-kit (called resources). The image shows a very simple dashboard for what it could look like. It is possible to add more advanced widgets.

![Dashboard](./assets/12-sample-dashboard.png)

### Start Programming!

It is now time to start programming the dev-kit. In the next chapters we will show you how.

## 4. Flash FiPy for NB-IoT or Update the LTE-M Firmware

Currently the FiPy comes with two separate modem firmware images. One image is for NB-IoT (NB1) and the other image is for LTE-M (Cat M1). Unfortunately only one of the modem images can be used at a time but we hope that this will change in the future, and that it will be possible to switch between the two network types in your own program for the FiPy without having to flash the modem.

From the factory the FiPy is configured with the LTE-M (Cat M1) modem firmware, so if you want to use the LTE-M network you do not have to flash the modem with new firmware and you can skip this chapter.

It could however be a good idea to upgrade the LTE-M firmware when new versions are released. Detailed instructions on how to update the LTE-M firmware or switch to NB-IoT firmware can be found here: https://docs.pycom.io/tutorials/lte/firmware.html

If you intend to use NB-IoT you must flash the modem firmware if you haven't done so already, as the factory image of the FiPy modem is configured for LTE-M (Cat M1).

## 5. Program the FiPy Using UDP (LTE-M or NB-IoT)

In this chapter you will learn how to program the FiPy dev-kit. The chapter will guide you through how to use the provided example code to connect the FiPy to the LTE-M or NB-IoT networks. Be aware that the FiPy Sequans modem firmware can only support either LTE-M (Cat M1) or NB-IoT. If you want to experiment with the NB-IoT network you will have to flash the Sequans modem on the FiPy with the NB-IoT firmware first as described in the previous chapter.

### Download Example Code

You can download the example code from here: https://github.com/TelenorStartIoT/fipy-dev-kit-udp

You should choose the "Download ZIP" option in the "Clone or Download" button pop-up. This will download a ZIP archive with the example code.

![Download ZIP](./assets/13-download-zip.png)

### Unzip the Example Code and Open It In VSCode

Unzip the example code. How to do this varies depending on your computer system. Most systems will unzip it if you double click on the zip file.

Open the folder using the "File > Open Folder" ("File > Open" on MacOS) option in VSCode.

![Open project in VSCode](./assets/14-open-project.png)

![Open project in VSCode](./assets/15-open-project.png)

### Modify the Program's Network Configuration

The program you downloaded is by default configured to use the LTE-M (Cat M1) network. If you open the [main.py](./main.py) file you will se the following code at the top:

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

### Run the Program

Connect the FiPy that is mounted on the expansion board to your computer (if not already). Make sure that the SIM and LTE antenna is connected! The Pymakr plugin in VSCode will automatically detect the dev-kit.

To upload and run the program on your FiPy, simply click the "Upload" button located at the bottom bar. This will first upload the code, then it will reset your FiPy and run the uploaded code.

### Check the Output From the Program

If everything goes well you should see output from the program in the Pycom Console window in VSCode. The image shows what it could look like (the output from the program might be different).

![Program output](./assets/16-program-output.png)

### Wrapping It All Up

You should now be able to view your data in the MIC dashboard. Try to add a time series widget by clicking the “+WIDGET” button in MIC and set the type to be "Time Series". Images below shows some sample screenshots on how to do it.

![Timeseries widget](./assets/17-timeseries-widget.png)

![Example dashboard](./assets/18-example-dashboard.png)

### Happy Hacking!

This concludes the "Get Started With the FiPy Dev-Kit (UDP over NB-IoT or LTE-M)" tutorial. Your next step could be to connect the supplied DHT11 sensor to the FiPy and to modify the "dummy" payload string with values from the DHT11 sensor.

A god starting point would be to use the library code supplied here:

https://github.com/JurassicPork/DHT_PyCom/tree/pulses_get

Happy hacking!

# Get Started With the EE R410

> Reading time: 45 minutes

The EE R410 breakout board is a pinout of the Ublox Sara R410M modem with power circuits, SIM card slot and an USB port. The breakout board does not include a processing unit and must be used together with an external unit that manages the modems serial port and issues AT commands (aka Hayes commands) to the modem in a controlled fashion. It connects to a PC via USB for easy experimentation, or via UART to embedded devices (an MCU or even a Raspberry Pi). It comes fully assembled with soldered headers and antenna.

More about the Ublox Sara R410M modem can be found here: https://www.u-blox.com/en/product/sara-r4n4-series

In this tutorial we will point you two examples on our experimental documentation site that shows how to use the breakout with either an Arduino based MCU sending UDP and JSON formatted payloads or a Raspberry Pi. The Raspberry Pi lesson will only show you how to get started using the UART connection to the modem but leaves the rest to you.
## Contents

  1. Chapter One
     1. Sub Header
     2. Sub Header 2
  2. Chapter Two
     1. Sub Header
     2. Sub Header 2

## 1. EE 410 breakout with Arduino

### Connecting the EE 410 to Horde

As explained in the Telenor Start IoT overview tutorial we have an experimental IoT platform called Horde. In order to connect the EE 410 breakout with an Arduino to Horde follow the steps found here:

https://docs.nbiot.engineering/tutorials/arduino-basic.html

It is of course also possible to connect the EE410 (with an Arduino) to the commercial Managed IoT Cloud platform. The next section will show you how.

### Connecting the EE410 to MIC
If you did connect the EE410 breakout to Horde (as per instructions above), first of all you will have to delete it from Horde. The reason for this is that MIC is integrated with Horde and will (using the Horde API) register the breakout with the same IMSI/IMEI when you follow the upcoming instructions.

**Register the breakout in MIC**

Use the IMEI/IMSI number of your breakout to add the breakout as a thing in MIC. How to do this is described in detail in this **MIC lesson.**

![ConnectingEEToMIC](https://github.com/TelenorStartIoT/tutorials/blob/master/05-ee-r410/%3D1-ConnectingEEToMIC.jpg)
Delete the device in IoTGateway(Horde) by clicking the “Delete” button.

**Change the hello.ino sketch**

Change the hello.ino sketch that was part of the Horde setup so that it sends payloads in JSON format to MIC

When you run the changed program you should now be able to see data in MIC as explained in the **MIC lesson** referenced above.

Change hello.ino from this
![ChangeHello.inoFrom](https://github.com/TelenorStartIoT/tutorials/blob/master/05-ee-r410/02-ChangeHello.inoFrom.jpg)

Change hello.ino to this
![ChangeHello.InoTO](https://github.com/TelenorStartIoT/tutorials/blob/master/05-ee-r410/03-ChangeHello.inoTo.jpg)


## 2. EE R410 breakout with Raspberry Pi

This tutorial will focus on how to set up, configure, and use serial ports (UART) to communicate with the breakout module. It will not cover how to connect to or configure the IP stack of your Raspberry Pi.

You can find the rest of this lesson on the experimental documentation site here:

https://docs.nbiot.engineering/tutorials/raspi-serial.html



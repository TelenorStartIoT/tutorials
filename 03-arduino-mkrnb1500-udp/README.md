# Get Started With the Arduino MKR NB 1500 Dev-Kit (UDP over NB-IoT or LTE-M)

> Reading time: 45 minutes

This tutorial gives brief instructions on how to get started with the Arduino MKR1500 dev kit. This tutorial will send data from the Arduino MKR1500 dev kit using JSON formatted data in a UDP packet over the NB1 (aka NB-IoT) or the M1 (aka LTE Cat M1 or LTE-M) network.

You will learn:

- How to download the Arduino Desktop IDE
- How to assemble the dev kit and connect it to the Arduino Desktop IDE
- How to create a Telenor Start IoT Managed IoT Cloud (MIC) platform account
- How to register your dev kit in MIC and create payload transformations
- How to program the dev kit and send data to MIC over Telenor’s excellent 4G LTE IoT Cat M1 network
- How to view your data in MIC

*You can also find a lot of info related to the Arduino MKR on Arduino´s own documentation site:*
https://www.arduino.cc/en/Guide/MKRNB1500Introduction...

## Contents

  1. [Preparations for Arduino dev kit](## 1. Preparations for Arduino dev kit)
     1. [Download and install the Arduino Desktop IDE](### Download and install the Arduino Desktop IDE)
     
  2. [Assemble the Arduino dev kit](## 2. Assemble the Arduino dev kit)
     1. [Connect the antenna and insert the SIM card](### Connect the antenna and insert the SIM card)
     2. [Add board support for the dev kit in the IDE](### Add board support for the dev kit in the IDE)
     3. [Select the board type in the IDE](### Select the board type in the IDE)
     4. [Select the port in the IDE](### Select the port in the IDE)
     5. [Get the IMSI and IMEI number of your dev kit](### Get the IMSI and IMEI number of your dev kit)
     
  3. [Register your Arduino dev kit in Telenor StartIoT Managed IoT Cloud](## 3.Register your Arduino dev kit in Telenor StartIoT Managed IoT Cloud)
     1. [Sign up for a MIC platform account](### Sign up for a MIC platform account)
     2. [Add a new thing type](### Add a new thing type)
     3. [Add a thing representing your dev kit](### Add a thing representing your dev kit)
     4. [See your thing](### See your thing)
     5. [Example dashboard](### Example dashboard)
     6. [Start Programming](### Start Programming)
 
 4. [Programming of the Arduino dev kit](## 4.Programming of the Arduino dev kit)
     1. [Download the example code as a zip file](### Download the example code as a zip file)
     2. [Add (open) the example code in the Arduino Desktop IDE](### Add (open) the example code in the Arduino Desktop IDE)
     3. [Add the MKRNB IoT library](### Add the MKRNB IoT library)
     4. [Run the example program](### Run the example program)
     5. [See your data displayed in MIC](### See your data displayed in MIC)
    
    
    
## 1. Preparations for Arduino dev kit

This lesson will show you how to download and install the Arduino Desktop IDE. The Arduino Desktop IDE is what you will use to connect to and program your dev kit.

### Download and install the Arduino Desktop IDE

The easiest way to program the Arduino MKR1500 dev kit is to use the Arduino Desktop IDE. You can download the Arduino IDE from https://www.arduino.cc/en/Guide/HomePage. Scroll down to the “Install the Arduino Desktop IDE” and select the link that is appropriate for your computers operating system and follow the instructions there.
     
![Download Aurduino desktop](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/01-%20Arduinohomepage.jpg)
     
When the Arduino Desktop IDE has been successfully installed you are ready to connect to the dev kit and start programming your own firmware for the Arduino. The next lesson will show you how to connect your dev kit to the Arduino IDE.
     
## 2. Assemble the Arduino dev kit

    
In this lesson you will learn how to assemble and connect the Arduino dev kit to the Arduino Desktop IDE. You will also communicate with the dev kit in order to check the firmware revision and retrieve the IMSI and IMEI numbers that you will need to register you dev kit in the Managed IoT Cloud platform. More on that in a later lesson, let us first connect the dev kit to the Arduino IDE.

### Connect the antenna and insert the SIM card

Before you connect the Arduino MKR1500 to your computer you need to make sure that the LTE antenna and the SIM card is installed onto the MKR1500 board. The antenna that comes with the dev kit should be mounted on the small UFL connector on the left side of the UBlox modem on the front side of the board. 

![ConnectingAntenna](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/02-Connecting-Antenna1.jpg)

The SIM card should be inserted in the SIM card slot located on the back side of the board. The small symbol on the SIM card slot shows the direction the SIM card should be inserted.

![SIMCardslot](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/03-SimCardSlot.jpg)

*Please also remove the black conductive foam from the MKR board pins before usage. If you don’t remove it, the board may behave erratically*

### Add board support for the dev kit in the IDE

Open the Arduino Desktop IDE. Before you connect your board to the computer, the first thing you need to do is to add the Atmel SAMD Core to the IDE. This simple procedure is done selecting Tools menu, then Boards and last Boards Manager. When the boards manager is displayed (see image), search for NB 1500 and install the SAMD core by clicking the install button

![AddBoardSupport](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/04-AddBoardSupport.jpg)

### Select the board type in the IDE

Now that the SAMD Core is installed, you can connect the board to the computer using a standard micro USB cable. In the IDE, from Tools select the Board Arduino MKR NB 1500.
![SelectingBoard](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/05-SelectingBoard.jpg)

### Select the port in the IDE

Now it is time to finally select the port the Arduino MKR1500 is connected to. This will look slightly different depending on the operating system your computer is using. The example image shows what it typically looks like on a Windows OS.

![SelectPortIDE](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/06-SelectPortID.jpg)

### Get the IMSI and IMEI number of your dev kit

Create a new sketch in the IDE and copy the following code into the sketch:

// baud rate used for both Serial ports
unsigned long baud = 115200;

void setup() {
  // enable the POW_ON pin
  pinMode(SARA_PWR_ON, OUTPUT);
  digitalWrite(SARA_PWR_ON, HIGH);
  // reset the ublox module
  pinMode(SARA_RESETN, OUTPUT);
  digitalWrite(SARA_RESETN, HIGH);
  delay(100);
  digitalWrite(SARA_RESETN, LOW);

  Serial.begin(baud);
  SerialSARA.begin(baud);
}

void loop() {
  if (Serial.available()) {
    SerialSARA.write(Serial.read());
  }

  if (SerialSARA.available()) {
    Serial.write(SerialSARA.read());
  }
}

![GettingIMSInumber](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/07A-GettingIMSInumber.jpg)

AT+CIMI;+CGSN

Leave the serial monitor open. You’ll need to copy these numbers when we provision the device in Telenor Start IoT Managed IoT Cloud in the next lesson.

In the next lesson you will register and connect your dev kit to Telenor Start IoT Managed IoT Cloud
![SerialMonitor](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/07B-GettingIMSInumber.jpg)

## 3.Register your Arduino dev kit in Telenor StartIoT Managed IoT Cloud

In this lesson you will learn how to register your dev kit to Telenor StartIoT Managed IoT Cloud (MIC). You will also learn ho to get the IMSI and IMEI number from the Arduino device. Using the IMSI and IMEI information you will learn how to add a new 4G NB-IoT thing in MIC. You will also learn how to add a transformation of your payload and to how to create a dashboard. The payload transformation makes it possible for you to view the individual parts of the payload in different type of widgets in the dashboard. Widget types ranges from simple textual widgets to graphical representations of your data.

### Sign up for a MIC platform account

You will have to register for a MIC account in order to register your dev kit. You can do that here:
https://startiot.mic.telenorconnexion.com

![MICSIGNUP](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/08-MICsignup.jpg)

Click on the “Sign Up” button in the upper right corner and follow the instructions in order to sign up. You should be aware that the signup is a two phased sign up. It therefore requires that you, in phase one, verify your email. We will send a link to the email you register and you will have to use the link to verify your email address. In phase two, we will manually register your private MIC domain and activate your account. You will then receive a second email stating that your account has been activated. Because of this procedure it may take up to 24 hours before your account is ready to use.

### Add a new thing type

You will now have to login to your MIC account when it is ready for use. When logged in you must create a new “thing type” for your dev kit. To add a new “thing type” click on the “+NEW THING TYPE button and fill in the form. Add the following code in the “Uplink transform” field:

return JSON.parse(payload.toString("utf-8"));

This code is just one simple example of what the uplink transform can look like. In this case it will transform JSON formatted payloads into its separate parts. For each part a resource in MIC will be created. A resource is an MQTT endpoint. Do not worry about the details now, this was just for your information. It is possible to create uplink transformations for payloads formatted in basically any format (hex, binary, text, JSON, etc). The uplink transform is just a snippet of Javascript code that MIC will use when doing transformations on your payload.

![AddingnewThing](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/08A-Addingnewthing.jpg)

### Add a thing representing your dev kit

You should now add a new “thing” for your dev kit. The “thing type” and “thing” together is a representation of your dev kit in MIC. It is possible to have more than one thing in a thing type and this will make the things in the thing type to behave in the same manner with respect to how payloads from the things are handled. The handling of the payload is described in your uplink transform. You must click on the +THINGS button to create a new thing. In the create new thing form, deselect the “Create batch” slider. You must then add a “Thing Name”, a “Description” and select your “Domain” and choose “Protocol” for your thing. When you select nbiot as protocol you will also have to add the IMSI and IMEI number of your dev kit. The IMSI and IMEI was obtained in a previous lesson. The image on the right shows an example of what it should look like.

![Addthing](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/08A-Addingnewthing.jpg)

### See your thing


You can look at and access your thing if you click the “List” tab. The image on the right shows an example list of devices reflecting a single dev kit thing.

![SeeYourThing](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/08C-Seething.jpg)


### Example dashboard

If you click on the “Thing name” in the list you will create a dashboard for your thing. The dashboard will be mainly empty until the first payload for your thing arrives. The dashboard is configurable and you can add widgets that represents values sent from your dev kit (called resources). The image on the right side shows a very simply dashboard for the dummy payload sent from your device. It is possible to add more advanced widgets. Play around!

![ExampleDashBoard](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/08D-ExampleDashBoard.jpg)

### Start Programming

It is now time to start programming the dev kit to send data over the LTE IoT network to MIC. In the next lesson we will show you how.

## 4.Programming of the Arduino dev kit

In this lesson you will learn how to program the Arduino dev kit. The lesson will guide you through how to use the provided example code to connect the Arduino dev kit to the LTE NB-IoT network. The example code prepares and sends dummy messages to Managed IoT Cloud over the LTE NB-IoT network.

### Download the example code as a zip file

You can download the example code from here:

https://github.com/TelenorStartIoT/ArduinoDevKitM1

You should choose the “Download ZIP” option in the “Clone or Download” button pop-up. This will download a zip file with the example code. Unzip the code in a folder on your machine.

![DownloadExampleCode](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/09-DownloadExampleCode.jpg)

### Add (open) the example code in the Arduino Desktop IDE

Unzip the example code. How to do this varies depending on your computer system. Most systems will unzip it if you double click on the zip file.

![AddExampleCode](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/09A-AddExampleCode.jpg)

Add the example code to the Arduino Desktop IDE (File->Open…) and select the main.ino file

### Add the MKRNB IoT library

The example code requires the Arduino MKRNB library. Add it to your sketch (Sketch->Include Library…), search for MKR NB and click install.

![AddMKRNBLibrary](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/09B-AddMKRNBLibrary.jpg)

### Run the example program

Compile and run the example code on the MKR1500 device by clicking on the upload arrow symbol or choosing “Sketch->Upload..” from the menu. Open the Serial Monitor (Tools->Serial Monitor…) and see the log output from your program.

![RunExampleProgram](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/09B-AddMKRNBLibrary.jpg)

### See your data displayed in MIC

Open the MIC dashboard and see your data displayed in MIC.

![MICDashBoard](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/09D-MICDashBoard.jpg)

### Happy hacking!

This concludes the Get started with the Arduino dev kit tutorial. Your next step could be to connect the supplied DHT11 sensor to the Arduino dev kit and to modify the “dummy” payload string with values from the DHT11 sensor.

A god starting point would be to find out how to use the DHT library code supplied here:

https://github.com/winlinvip/SimpleDHT

Happy hacking!



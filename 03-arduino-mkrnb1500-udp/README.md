# Arduino Dev-Kit - UDP

> Works with NB-IoT or LTE-M

This tutorial gives brief instructions on how to get started with the Arduino MKR NB 1500 dev-kit. This tutorial will send JSON formatted data in a UDP packet over the LTE-M (Cat M1) or NB-IoT (NB1) network.

You will learn how to:

  * download the Arduino Desktop IDE
  * assemble the dev-kit and connect it to the Arduino Desktop IDE
  * create a Telenor Managed IoT Cloud (MIC) platform account
  * register your dev-kit in MIC and create payload transformations
  * program the dev-kit and send data to MIC over Telenor's excellent 4G LTE-M or NB-IoT network
  * view your data in MIC

You can also find a lot of info related to the Arduino MKR NB 1500 on Arduino's own documentation site:
https://www.arduino.cc/en/Guide/MKRNB1500Introduction...

## 1. Download and Install the Arduino Desktop IDE and Add Board Support

This lesson will show you how to download and install the Arduino Desktop IDE and add board support for the Arduino MKR NB 1500 dev-kit. The Arduino Desktop IDE is what you will use to connect to and program your dev-kit.

### 1.1 Download the Arduino Desktop IDE

The easiest way to program the Arduino MKR NB 1500 dev-kit is to use the Arduino Desktop IDE. You can download the Arduino IDE for Windows, Linux and MacOS here: https://www.arduino.cc/en/Main/software.

![Download Aurduino Desktop IDE](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/assets/00-download-arduino-ide.png)

### 1.2 Add Board Support for the Dev-Kit in the IDE

Open the Arduino Desktop IDE. The first thing you need to do before you connect your board to the computer is to add the Atmel SAMD Core to the IDE. This simple procedure is done by selecting "Tools" menu, then "Boards" and "Boards Manager". When the boards manager is displayed (see image), search for "MKR NB" and install the SAMD core by clicking the install button.

![Add board support](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/assets/01-add-board-support.png)

### 1.3 Add the MKRNB Library

The example code that we will later run requires the Arduino MKRNB library. Add it to your sketch (Sketch > Include Library...), search for MKR NB and click install.

![Add MKRNB library](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/assets/02-add-library.png)

## 2. Assemble the Arduino Dev-Kit

In this chapter you will learn how to assemble and connect the Arduino dev-kit to the Arduino Desktop IDE. You will also communicate with the dev-kit in order to retrieve the IMSI and IMEI numbers that you will need to register you dev-kit in the MIC platform. More on that in a later chapter, let us first assemble the dev-kit.

### 2.1 Attach the Antenna and Insert the SIM Card

Before you connect the Arduino MKR NB 1500 to your computer you need to make sure that the LTE antenna and the SIM card is installed on the board. The antenna that comes with the dev-kit should be mounted on the small UFL connector on the left side of the UBlox modem on the front side of the board. 

![LTE antenna](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/assets/03-connect-antenna.png)

The SIM card should be inserted in the SIM card slot located on the back side of the board. The small symbol on the SIM card slot shows the direction the SIM card should be inserted.

![SIM card slot](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/assets/04-sim-card-slot.png)

Please also remove the black conductive foam from the board pins before usage. If you don't remove it, the board may behave erratically.

### 2.2 Select the Board Type in the IDE

If the SAMD Core is installed you can connect the board to the computer using a standard micro USB cable. In the IDE, from "Tools" menu select the Board "Arduino MKR NB 1500".

![Select board](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/assets/05-select-board.png)

### 2.3 Select the Port in the IDE

Now it is time to finally select the port that the Arduino MKR NB 1500 is connected to. This will look slightly different depending on the operating system your computer is using. The example image shows what it typically looks like on MacOS.

![Select port](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/assets/06-select-port.png)

### 2.4 Get IMSI and IMEI

Create a new sketch in the IDE and copy the following code into the sketch:

``` cpp
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
```

This will allow us to send commands from the PC directly to the modem on the Arduino board. We will do this to get the IMSI and IMEI numbers that we'll need later on. 

![Serial passthrough sketch](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/assets/07-serial-passthrough.png)

Open the serial monitor (Tools > Serial Monitor). Make sure that the "Both NL & CR" option is selected and that the baud rate is set to "115200 baud". Then, type the following command in the input field to get the IMSI and IMEI numbers:

```
AT+CIMI;+CGSN
```

Leave the serial monitor open. You'll need to copy these numbers when we provision the device in Telenor Managed IoT Cloud in the next chapter.

![Serial monitor](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/assets/08-get-imsi-imei.png)

## 3. Register Your Arduino Dev-Kit in Telenor Managed IoT Cloud

In this chapter you will learn how to register your dev-kit to Telenor Managed IoT Cloud (MIC). Using the IMSI and IMEI information you will learn how to add a new "Thing" in MIC. You will also learn how to add a transformation of your payload and how to create a dashboard. The payload transformation makes it possible for you to unpack data packets and view the individual parts of the payload in different types of widgets in the dashboard. Widget types ranges from simple textual widgets to graphical representations of your data.

### 3.1 Sign Up for a MIC Platform Account

You will have to register for a MIC account in order to register your dev-kit. You can do that here: https://demonorway.mic.telenorconnexion.com

![Login](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/assets/09-login-mic.png)

Click on the "Sign Up" button in the upper right corner and follow the instructions in order to sign up. You should be aware that the signup is a two phased sign up. It therefore requires that you, in phase one, verify your email. We will send a link to the email you register and you will have to use the link to verify your email address. In phase two, we will manually register your private MIC domain and activate your account. You will then receive a second email stating that your account has been activated. Because of this procedure it may take up to 24 hours before your account is ready to be used.

### 3.2 Add a New Thing Type

You will be able to login to your MIC account when it is ready for use. When logged in create a new "Thing Type" for your dev-kit. A "Thing Type" is a way to organize multiple "Things" that share similarities.

To add a new "Thing Type" click on the "+NEW THING TYPE" button and give it a name and a description. Assign it to the domain that your user was added to. Finally, in the "Uplink Transform" add the following code:

``` js
return JSON.parse(payload.toString('utf-8'));
```

![Create Thing Type](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/assets/10-thing-type.png)

This code is just one simple example of what the uplink transform can look like. In this case it will transform JSON formatted payloads into a JavaScript object and return it. This will separate each property of the object into its separate "parts". For each "part" a resource in MIC will be created.

Do not worry about the details for now, this was just for your information. It is possible to create uplink transformations for payloads formatted in basically any format (hex, binary, text, JSON, etc.). The uplink transform is just a snippet of JavaScript code that MIC will use when doing transformations on your payload, and is generally used to unpack small payloads into understandable resources.

### 3.3 Add a Thing Representing Your Dev-Kit

The "Thing Type" and "Thing" together is a representation of your dev-kit in MIC. It is possible to have more than one Thing in a Thing Type and this will make the Things in the Thing Type behave in the same manner with respect to how payloads from the Things are handled. The handling of the payload is described in your uplink transformation.

To add a new Thing, click on the "+ THINGS" button.

![Add new Thing to Thing Type](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/assets/11-new-thing.png)

A pop-up window will appear. De-select the "Create batch" slider in the pop-up window. You must then add a "Thing Name", a "Description", select your "Domain" and choose "Protocol" for your Thing. When you select "LPWAN" as protocol you will also have to add the IMSI and IMEI numbers of your dev-kit. The image shows an example of what it should look like.

![Add new Thing to Thing Type](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/assets/12-thing.png)

### 3.4 See Your Newly Created Thing

You can look at and access your Thing if you click the "List" tab. The image shows an example list of devices reflecting a single dev-kit Thing.

![New Thing in MIC](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/assets/13-thing-list-widget.png)

The Thing List widget will show our single Thing that we just created.

![New Thing in MIC](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/assets/14-thing-list.png)

### 3.5 Example Dashboard

If you click on the "Thing name" in the list you will create a dashboard for your Thing. The dashboard will be mainly empty until the first payload for your Thing arrives. The dashboard is configurable and you can add widgets that represents values sent from your dev-kit (called resources). The image shows a very simple dashboard for what it could look like. It is possible to add more advanced widgets.

![Sample Dashboard](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/assets/14-sample-dashboard.png)

### 3.6 Start Programming!

It is now time to start programming the dev-kit. In the next chapters we will show you how.

## 4. Programming of the Arduino Dev-Kit

In this chapter you will learn how to program the Arduino dev-kit. The chapter will guide you through how to use the provided example code to connect the Arduino dev-kit to the LTE-M (Cat M1) or NB-IoT network. The example code prepares and sends dummy messages to Telenor Managed IoT Cloud.

### 4.1 Download Example Code

You can download the example code from here: https://github.com/TelenorStartIoT/arduino-dev-kit-udp

You should choose the "Download ZIP" option in the "Clone or Download" button pop-up. This will download a ZIP archive with the example code.

![Download ZIP](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/assets/14-download-zip.png)

### 4.2 Unzip the Example Code and Open It In Arduino Desktop IDE

Unzip the example code. How to do this varies depending on your computer system. Most systems will unzip it if you double click on the zip file.

Add the example code to the Arduino Desktop IDE (File > Open...) and select the main.ino file.

![Open project in Arduino Desktop IDE](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/assets/15-open-project.png)

![Open project in Arduino Desktop IDE](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/assets/16-open-project2.png)


### 4.3 Run the Program

Compile and run the example code on the Arduino MKR NB 1500 device by clicking on the upload arrow symbol or choosing "Sketc" > "Upload..." from the menu. Open the Serial Monitor (Tools > Serial Monitor...) and see the log output from your program.

![Run the program](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/assets/17-run-program.png)

![Run the program](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/assets/18-run-program2.png)

### 4.4 See Your Data Displayed in MIC

Open the MIC dashboard and see your data displayed in MIC.

![Example dashboard](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-udp/assets/19-example-dashboard.png)

### 4.5 Happy Hacking!

This concludes the "Get Started With the Arduino Dev-Kit (UDP over NB-IoT or LTE-M)" tutorial. our next step could be to connect the supplied DHT11 sensor to the FiPy and to modify the "dummy" payload string with values from the DHT11 sensor.

A god starting point would be to use the library code supplied here:

https://github.com/winlinvip/SimpleDHT

Happy hacking!

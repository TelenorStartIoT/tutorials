# Arduino Dev-Kit - CoAP

> Works with NB-IoT or LTE-M

This tutorial gives brief instructions on how to get started with the Arduino MKR NB 1500 dev-kit. This tutorial will send JSON formatted data in a CoAP packet over the LTE-M (Cat M1) or NB-IoT (NB1) network.

You will learn how to:

1. Sign up for a Managed IoT Cloud (MIC) platform account
2. Download the software you need
3. Assemble the dev-kit and connect it to the Arduino Desktop IDE

- create a Telenor Managed IoT Cloud (MIC) platform account
- register your dev-kit in MIC and create payload transformations
- program the dev-kit and send data to MIC over Telenor's excellent 4G LTE-M or NB-IoT network
- view your data in MIC

You can also find a lot of info related to the Arduino MKR NB 1500 on Arduino's own documentation site: https://www.arduino.cc/en/Guide/MKRNB1500

## 1. Sign Up For a MIC Platform Account

If you already have a MIC account you can start directly at step 2.

You can **Sign-up** for a MIC account here: https://demonorway.mic.telenorconnexion.com

![Login](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/09-login-mic.png)

Click on the "Sign Up" button in the upper right corner and follow the instructions in order to sign up. It is important that you fill out at least the **six first fields** in addition to **company**. This is a prerequisite for being verified as a user.

**Note:** You should be aware that the signup is two-phased, and after you have confirmed your email address it may take some time before your user account is activated. This is a manual verification process and may take up to 24 hours outside normal business hours. Once your account is ready for use you will receive an email stating that your account has been activated.
You can however continue.

## 2. Download the software you need

To connect and program the Arduino MKR NB 1500 dev-kit you need to download and install:

- Arduino Desktop IDE
- Add Board Support
- Add MKRNB Library

If you already have these programs on your computer you can jump straight to step 3.

### 2.1 Arduino Desktop IDE

The easiest way to program the Arduino MKR NB 1500 dev-kit is to use the Arduino Desktop IDE. You can download the Arduino IDE for Windows, Linux and macOS here: https://www.arduino.cc/en/Main/software.

![Download Aurduino Desktop IDE](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/00-download-arduino-ide.png)

### 2.2 Add Board Support

Open the Arduino Desktop IDE. The first thing you need to do before you connect your board to the computer is to add the Atmel SAMD Core to the IDE. This simple procedure is done by selecting "Tools" menu, then "Boards" and "Boards Manager". When the boards manager is displayed (see image), search for "MKR NB" and install the SAMD core by clicking the install button.

![Add board support](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/01-add-board-support.png)

### 2.3 Add MKRNB Library

The example code that we will later run requires the Arduino MKRNB library. Add it to your sketch (Sketch > Include Library...), search for MKR NB and click install.

![Add MKRNB library](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/02-add-library.png)

You now have everything in place to connect and start programming your Arduino MKR NB 1500. The next chapter will show you how to assemble your Arduino dev-kit.

## 3. Assemble the Arduino Dev-Kit

In this chapter you will learn how to assemble and connect the Arduino dev-kit to the Arduino Desktop IDE. You will also communicate with the dev-kit in order to retrieve the IMSI and IMEI numbers that you will need to register you dev-kit in the MIC platform. More on that in a later chapter, let us first assemble the dev-kit.

### 3.1 Attach the Antenna and Insert the SIM Card

Before you connect the Arduino MKR NB 1500 to your computer you need to make sure that the LTE antenna and the SIM card is installed on the board. The antenna that comes with the dev-kit should be mounted on the small UFL connector on the left side of the UBlox modem on the front side of the board.

![LTE antenna](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/03-connect-antenna.png)

The SIM card should be inserted in the SIM card slot located on the back side of the board. The small symbol on the SIM card slot shows the direction the SIM card should be inserted.

![SIM card slot](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/04-sim-card-slot.png)

Please also remove the black conductive foam from the board pins before usage. If you don't remove it, the board may behave erratically.

### 3.2 Select the Board Type in the IDE

If the SAMD Core is installed you can connect the board to the computer using a standard micro USB cable. In the IDE, from "Tools" menu select the Board "Arduino MKR NB 1500".

![Select board](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/05-select-board.png)

### 3.3 Select the Port in the IDE

Now it is time to finally select the port that the Arduino MKR NB 1500 is connected to. This will look slightly different depending on the operating system your computer is using. The example image shows what it typically looks like on MacOS.

![Select port](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/06-select-port.png)

### 3.4 Get IMSI and IMEI

Create a new sketch in the IDE and copy the following code into the sketch:

```cpp
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

![Serial passthrough sketch](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/07-serial-passthrough.png)

Open the serial monitor (Tools > Serial Monitor). Make sure that the "Both NL & CR" option is selected and that the baud rate is set to "115200 baud". Then, type the following command in the input field to get the IMSI and IMEI numbers:

```
AT+CIMI;+CGSN
```

Leave the serial monitor open. You'll need to copy these numbers when we provision the device in Telenor Managed IoT Cloud in the next chapter.

![Serial monitor](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/08-get-imsi-imei.png)

## 4. Register Your Arduino Dev-Kit in Telenor Managed IoT Cloud as a CoAP Thing

In this chapter you will learn how to register your dev-kit to Telenor Managed IoT Cloud (MIC). Using the IMSI and IMEI information you will learn how to add a new "Thing" in MIC. You will also learn how to add a transformation of your payload and how to create a dashboard. The payload transformation makes it possible for you to unpack data packets and view the individual parts of the payload in different types of widgets in the dashboard. Widget types ranges from simple textual widgets to graphical representations of your data.

### 4.1 Create a "Thing Type" for Your Dev-Kit

Once your MIC account is activated, you will be able to login to your MIC account with your user id and password.

When you log in for the first time you can start by creating a new **Thing Type**.

- A **Thing Type** is a method of organising multiple "Things" that report on the same data resources.
- A **Thing** must always belong to a **Thing Type**, this is why we have to make this before registering a "Thing".
- In the "Uplink Transform" add the following code:

```js
return JSON.parse(payload.toString("utf-8"));
```

![Create Thing Type](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/10-thing-type.png)

This code is just one simple example of what the uplink transform can look like. In this case it will transform JSON formatted payloads into a JavaScript object and return it. This will separate each property of the object into its separate "parts". For each "part" a resource in MIC will be created.

Do not worry about the details for now, this was just for your information. It is possible to create uplink transformations for payloads formatted in basically any format (hex, binary, text, JSON, etc.).

The uplink transform is just a snippet of JavaScript code that MIC will use when doing transformations on your payload, and is generally used to unpack compressed payloads into understandable resources.

### 4.2 Add a "Thing" Representing Your Dev-Kit

Once you have established a **Thing Type** you can add your dev-kit as a **Thing**.

Your **Thing Types** will be visible to you in the panel on the left hand side of the window. When creating a new **Thing** it will automatically belong to the selected **Thing Type**.

To add a new Thing, click on the "+ THINGS" button.

![Add new Thing to Thing Type](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/11-new-thing.png)

A pop-up window will appear.

- De-select the "**Create batch**" slider in the pop-up window.
- You must then add a "**Thing Name**", a "**Description**" and select your "**Domain**" from the drop-down menu.
- Choose "**nbiot**" (or LPWAN/CoAP) as the protocol for your Thing. You will also have to add the IMSI and IMEI numbers that we got earlier.

The image shows an example of what it should look like.

![Add new Thing to Thing Type](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/12-thing.png)

The things list will now show the Thing that you just created.

![New Thing in MIC](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/14-thing-list.png)

You are now finished registering your thing in MIC, and you can start working with the dev-kit again.

### 4.3 Example Dashboard

If you click on the "Thing name" in the list you will create a dashboard for your Thing. The dashboard will be mainly empty until the first payload for your Thing arrives. The dashboard is configurable and you can add widgets that represents values sent from your dev-kit (called resources). The image shows a very simple dashboard for what it could look like. It is possible to add more advanced widgets.

![Sample Dashboard](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/14-sample-dashboard.png)

### 4.4 Start Programming!

It is now time to start programming the dev-kit. In the next chapters we will show you how.

## 5. Program the Arduino Using CoAP

In this chapter you will learn how to program the Arduino dev-kit to send data using CoAP directly to MIC. The chapter will teach you how to use the provided example code to connect the Arduino to the LTE-M or NB-IoT networks and publish dummy messages directly to Telenor Managed IoT Cloud.

The chapter will take you through these steps:

- Download and Save the Example Code from GitHub
- Open the code in VSCode
- Modify the Program's Network Configuration
- Upload the Program to Your Arduino
- Check the Output of the Program

### 5.1 Download Example Code

You can download the example code from here: https://github.com/TelenorStartIoT/arduino-dev-kit-udp

You should choose the "Download ZIP" option in the "Clone or Download" button pop-up. This will download a ZIP archive with the example code.

![Download ZIP](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/14-download-zip.png)

Unzip the example code. How to do this varies depending on your computer system. Most systems will unzip it if you double click on the zip file.

Give the folder a "_name you can remember_" and save it to someplace you can find back to it.

### 5.2 Open the Code in Arduino Desktop IDE

The folder you created in the previous step contains everything you need for your project code. Add the example code to the Arduino Desktop IDE (File > Open...) and select the main.ino file.

![Open project in Arduino Desktop IDE](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/15-open-project.png)

![Open project in Arduino Desktop IDE](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/16-open-project2.png)

### 5.3 Modify the Program's Network Configuration

The program you downloaded is by default configured to use the LTE-M network. If you open the [arduino_secrets.h](https://github.com/TelenorStartIoT/arduino-dev-kit-udp/blob/master/arduino_secrets.h) file you will se the following code:

```cpp
// Network related configuration
#define SECRET_PINNUMBER     ""              // SIM card PIN number
#define SECRET_GPRS_APN      "telenor.iotgw" // Telenor IoT Gateway APN
#define SECRET_COAP_IP       0x012010AC      // Telenor IoT Gateway IP address (172.16.32.1)
#define SECRET_COAP_ENDPOINT "/"             // Telenor IoT Gateway CoAP endpoint
#define SECRET_COAP_PORT     5683            // Telenor IoT Gateway CoAP port
#define SECRET_RAT           7               // Radio Access Technology (7 is for LTE-M and 8 is for NB-IoT)
#define SECRET_COPS          24201           // Telenor network shortname
```

As described in the code comments, if you are using the NB-IoT network you must change the `SECRET_RAT` define to `8`:

```cpp
#define SECRET_RAT           8               // Radio Access Technology (7 is for LTE-M and 8 is for NB-IoT)
```

### 5.4 Run the Program

Compile and run the example code on the Arduino MKR NB 1500 device by clicking on the upload arrow symbol or choosing "Sketch" > "Upload..." from the menu. Open the Serial Monitor (Tools > Serial Monitor...) and see the log output from your program.

![Run the program](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/17-run-program.png)

![Run the program](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/18-run-program2.png)

## 6. Visualize Your Data in MIC

Open the MIC dashboard and see your data displayed in MIC.

![Example dashboard](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/19-example-dashboard.png)

### 6.1 Happy Hacking!

This concludes the "Get Started With the Arduino Dev-Kit (CoAP over NB-IoT or LTE-M)" tutorial. our next step could be to connect the supplied DHT11 sensor to the Arduino and to modify the "dummy" payload string with values from the DHT11 sensor.

Happy hacking!

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

  1. Preparations for Arduino dev kit
     1. Download and install the Arduino Desktop IDE
     
  2. Assemble the Arduino dev kit
     1. Connect the antenna and insert the SIM card
     2. Add board support for the dev kit in the IDE
     3. Select the board type in the IDE
     4. Select the port in the IDE
     5. Get the IMSI and IMEI number of your dev kit
    
## 1. Preparations for Arduino dev kit

This lesson will show you how to download and install the Arduino Desktop IDE. The Arduino Desktop IDE is what you will use to connect to and program your dev kit.

### Download and install the Arduino Desktop IDE

The easiest way to program the Arduino MKR1500 dev kit is to use the Arduino Desktop IDE. You can download the Arduino IDE from https://www.arduino.cc/en/Guide/HomePage. Scroll down to the “Install the Arduino Desktop IDE” and select the link that is appropriate for your computers operating system and follow the instructions there.
     
![Download Aurduino desktop](./01-%20Arduino%20home%20page.png)
     
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


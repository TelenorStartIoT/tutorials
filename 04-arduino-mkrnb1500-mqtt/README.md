# Get Started With the Arduino MKR NB 1500 Dev-Kit (MQTT/TLS 1.2 over LTE-M)

> LTE-M

**NB! This code example is for prototyping only. Not for deployment in production.**

# Arduino MKR1500 MQTT/TLS 1.2 over M1
This tutorial gives brief instructions on how to get started with the Arduino MKR1500 dev kit. This tutorial will send data from the Arduino MKR1500 dev kit using secure (TLS 1.2) MQTT packet over the M1 (LTE Cat M1) network.

You will learn:

* How to download the Arduino Desktop IDE
* How to assemble the dev kit and connect it to the Arduino Desktop IDE
* How to create a Telenor Start IoT Managed IoT Cloud (MIC) platform account
* How to register your dev kit in MIC and create payload transformations
* How to program the dev kit and send data to MIC over MQTT/TLS1.2 on Telenor’s excellent 4G LTE IoT Cat M1 network
* How to view your data in MIC

You can also find a lot of info related to the Arduino MKR NB 1500 on Arduino's own documentation site:
https://www.arduino.cc/en/Guide/MKRNB1500Introduction

## 1. Download and Install the Arduino Desktop IDE and Add Board Support

This lesson will show you how to download and install the Arduino Desktop IDE and add board support for the Arduino MKR NB 1500 dev-kit. The Arduino Desktop IDE is what you will use to connect to and program your dev-kit.

### 1.1 Download the Arduino Desktop IDE

The easiest way to program the Arduino MKR NB 1500 dev-kit is to use the Arduino Desktop IDE. You can download the Arduino IDE for Windows, Linux and MacOS here: https://www.arduino.cc/en/Main/software.

![Download Aurduino Desktop IDE](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/00-download-arduino-ide.png)

### 1.2 Add Board Support for the Dev-Kit in the IDE

Open the Arduino Desktop IDE. The first thing you need to do before you connect your board to the computer is to add the Atmel SAMD Core to the IDE. This simple procedure is done by selecting "Tools" menu, then "Boards" and "Boards Manager". When the boards manager is displayed (see image), search for "MKR NB" and install the SAMD core by clicking the install button.

![Add board support](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/01-add-board-support.png)

### 1.3 Add the MKRNB Library

The example code that we will later run requires the Arduino MKRNB library. Add it to your sketch (Sketch > Include Library...), search for MKR NB and click install.

![Add MKRNB library](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/02-add-library.png)

## 2. Assemble the Arduino Dev-Kit

In this chapter you will learn how to assemble and connect the Arduino dev-kit to the Arduino Desktop IDE. You will also communicate with the dev-kit in order to retrieve the IMSI and IMEI numbers that you will need to register you dev-kit in the MIC platform. More on that in a later chapter, let us first assemble the dev-kit.

### 2.1 Attach the Antenna and Insert the SIM Card

Before you connect the Arduino MKR NB 1500 to your computer you need to make sure that the LTE antenna and the SIM card is installed on the board. The antenna that comes with the dev-kit should be mounted on the small UFL connector on the left side of the UBlox modem on the front side of the board. 

![LTE antenna](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/03-connect-antenna.png)

The SIM card should be inserted in the SIM card slot located on the back side of the board. The small symbol on the SIM card slot shows the direction the SIM card should be inserted.

![SIM card slot](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/04-sim-card-slot.png)

Please also remove the black conductive foam from the board pins before usage. If you don't remove it, the board may behave erratically.

### 2.2 Select the Board Type in the IDE

If the SAMD Core is installed you can connect the board to the computer using a standard micro USB cable. In the IDE, from "Tools" menu select the Board "Arduino MKR NB 1500".

![Select board](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/05-select-board.png)

### 2.3 Select the Port in the IDE

Now it is time to finally select the port that the Arduino MKR NB 1500 is connected to. This will look slightly different depending on the operating system your computer is using. The example image shows what it typically looks like on MacOS.

![Select port](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/06-select-port.png)

### 2.4 Get IMSI and IMEI

Create a new sketch in the IDE and copy the following code into the sketch:

``` cpp
// baud rate used for both Serial ports
unsigned long baud = 115200;

void setup() {
  // enable the POW_ON pin
  pinMode(SARA_PWR_ON, OUTPUT);
  digitalWrite(SARA_PWR_ON, HIGH);
  
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

This will allow us to send commands from the PC directly to the modem on the Arduino board. We will do this to get the IMSI and IMEI numbers that we'll need later on. Press the "check" sign in the top left corner to verify the code, then press the arrow next to it to upload the code to your device.

![Serial passthrough sketch](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/07-serial-passthrough.png)

Open the serial monitor (Tools > Serial Monitor). Make sure that the "Both NL & CR" option is selected and that the baud rate is set to "115200 baud". Then, type the following command in the input field to get the IMSI and IMEI numbers:

```
AT+CIMI;+CGSN
```

Leave the serial monitor open. You'll need to copy these numbers when we provision the device in Telenor Managed IoT Cloud in the next chapter.

![Serial monitor](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/08-get-imsi-imei.png)


## 3.Register your Arduino dev kit in Telenor StartIoT Managed IoT Cloud

In this lesson you will learn how to register for an account in the Managed IoT Cloud (MIC) platform and register your device. 

### Sign up for a MIC platform account

If you already have a MIC account you can start directly at the next step. 

You can **Sign-up** for a MIC account here: https://demonorway.mic.telenorconnexion.com

![Login](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/07-login-mic.png)

Click on the "Sign Up" button in the upper right corner and follow the instructions in order to sign up. It is important that you fill out at least the **six first fields** in addition to **company**. This is a prerequisite for being verified as a user. 

**Note:** You should be aware that the signup is two-phased, and after you have confirmed your email address it may take some time before your user account is activated. This is a manual verification process and may take up to 24 hours outside normal business hours. Once your account is ready for use you will receive an email stating that your account has been activated. 

### Add a new thing type

Once your MIC account is activated, you will be able to login to your MIC account with your user id and password.

When you log in for the first time you can start by creating a new **Thing Type**. 
   * A **Thing Type** is a method of organising multiple "Things" that report on the same data resources. 
   * A **Thing** must always belong to a **Thing Type**, this is why we have to make this before registering a "Thing".


To add a *new* **Thing Typ** click on the **+NEW THING TYPE** button and give it a name and a description. 
* Assign it to your domain in the drop-down menu.  

Note: For things that will communicate with the **MQTT protocol** it is **not necessary to add uplink or downlink transformations** since the thing will send data following the MIC MQTT shadow update format. This format is directly understood by MIC. 

![Create Thing Type](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/08-thing-type.png)

For use cases where other protocols than MQTT is used, the uplink and downlink transforms will decide the payload handeling of all the **Things** within a **Thing Type**. This however is not relevant for this tutorial. 


### Add a thing representing your dev kit

Once you have established a **Thing Type** you can add your dev-kit as a **Thing**.

Your **Thing Types** will be visible to you in the panel on the left hand side of the window. When creating a new **Thing** it will automatically belong to the selected **Thing Type**.

To add a new Thing, click on the **"+ THINGS"** button.

![Add new Thing to Thing Type](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/09-add-new-thing.png)

A pop-up window will appear. 
   * De-select the **"Create batch"** slider in the pop-up window.
   * You must then add a **"Thing Name"**, a **"Description"** and select your **"Domain"** from the drop-down menu.
   * Choose **"MQTT"** as the protocol for your Thing.

The image shows an example of what it should look like.

![Configure new Thing in Thing Type](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/10-thing.PNG)

The things list will now show the Thing that you just created.

![New Thing in MIC](https://github.com/TelenorStartIoT/tutorials/blob/master/02-fipy-mqtt/assets/12-thing-list.png)


### Example dashboard

If you click on the “Thing name” in the list you will create a dashboard for your thing. The dashboard will be mainly empty until the first payload for your thing arrives. The dashboard is configurable and you can add widgets that represents values sent from your dev kit (called resources). The image on the right side shows a very simply dashboard for the dummy payload sent from your device. It is possible to add more advanced widgets. Play around!

![ExampleDashBoard](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/14-sample-dashboard.png)

### Start Programming

It is now time to start programming the dev kit to send data over the LTE IoT network to MIC. In the next lesson we will show you how.

## Programming the Arduino for MQTT over TLS 1.2 (M1 only)

Telenor StartIoT Managed IoT Cloud (our platform) is capable of handling MQTT publish/subscribe over a secure TLS 1.2 connection. The TLS 1.2 connection must be created with the usage of X.509 certificates and key. The Arduino MKR1500 dev kit modem (UBlox R410) has an onboard TLS stack and by using this stack it is possible to send data securely directly to MIC over the M1 network. Note that we have not been able to make it work on the NB1 network yet.

The needed certificate and key for your thing can be downloaded from Managed IoT Cloud and transformed into a configuration file for the Arduino sketch. Let us start with downloading the example code and adding it to the Arduino IDE.

## Download source code

Download the source code as a zip file from here:

https://github.com/TelenorStartIoT/arduino-dev-kit-mqtt

Unzip the source code. How to do the unzipping will vary depending on your operating system but on many systems it is enough to double click the zip file

![DownloadSourceCode](https://github.com/TelenorStartIoT/tutorials/blob/master/04-arduino-mkrnb1500-mqtt/assets/01-DownloadSourcecode.jpg)

## Add (open) the example code in the Arduino Desktop IDE

Add the example code to the Arduino Desktop IDE (File->Open…) and select the Arduino_MIC_MQTT.ino file. We do want to change the content of the MICCertificates——–.h file with the certificate and private key for your thing in MIC. Let us first download the certificate and key from MIC.

![AddExampleSourceCode](https://github.com/TelenorStartIoT/tutorials/blob/master/04-arduino-mkrnb1500-mqtt/assets/02-ExampleCode.jpg)

## Download MIC certificate and keys

You will have to download the MIC certificate for your thing. It will be downloaded as a zip file. Unzip the zip file. Unzipping the zip file creates a folder with the certificates and keys. You will have to transform the certificates and the private key to DER Hex format since this is the only format the UBlox R410 modem understands. In order to facilitate for mutual authentication between the dev kit and the platform you will also need the AWS IoT root certificate. This certificate is already included the Arduino source code.

![MICCertifiacted](https://github.com/TelenorStartIoT/tutorials/blob/master/04-arduino-mkrnb1500-mqtt/assets/03-MICCertificated.jpg)

## Change file name and include statement in the .ino file

The goal is to create a MICCertificates——–.h file containing your things certificates and keys. The file MICCertificatesYOUR_THING_ID.h is just a template. As a start, rename this file to match your things thing id by changing the YOUR_THING_ID part of the file name with your things id (e.g. MICCertificates00001234.h). Also change the include statement in the .ino file accordingly. If your SIM card has a PIN you will also have to add that in the .ino file.

![ChangeFileName](https://github.com/TelenorStartIoT/tutorials/blob/master/04-arduino-mkrnb1500-mqtt/assets/04-ChangeFileName.jpg)

## Transforming the certificate and key

*NOTE: When following the next two steps to transform the PEM client certificate and the PEM private key, the needed parameters to and output from openssl commands and hexdump may vary with different versions of openssl, hexdump and operating systems. If you experience differences you will have to try your best 🙂 to get it right.*

## Transform the PEM client certificate

The certificate and private key downloaded from MIC is in PEM format. The UBlox R410 modem expects the certificate and key to be in DER format. To transform the content of your thing´s PEM certificate to DER Hex format you can use the following command:

MAC/Linux:

`openssl x509 -C -in cert.pem > certDER.c`

Windows (you may have to download and install the openssl package found [here](http://slproweb.com/download/Win64OpenSSL_Light-1_1_1d.exe)):

`& 'C:\Program Files\OpenSSL-Win64\bin\openssl.exe' rsa -inform PEM -in privkey.pem -outform DER -out privkey.dat`

where the first filepath is where the openssl package is installed, this is usually as in the example or 'C:\OpenSSL-Win64\'

Open the resulting certDER.c file and scroll down to the “unsigned char the_certificate[xxx]” part of the file. Copy the Hex for the certificate into your MICCertificates——–.h file and add the number (xxx) at the bottom of the definition (this is the size).

Copy the certificate hex code from the generated certDER.c

![CopyHexCode](https://github.com/TelenorStartIoT/tutorials/blob/master/04-arduino-mkrnb1500-mqtt/assets/05-CopyHexCode.jpg)

Copy the certificate hex code to the MIC_CLIENT_CERTIFICATE part of the MICCertificates——–.h file

![CopyHexCodeCertToMIC](https://github.com/TelenorStartIoT/tutorials/blob/master/04-arduino-mkrnb1500-mqtt/assets/06-CopyHexCodeCertToMIC.jpg)

Add the size number at the bottom of the MIC_CLIENT_CERTIFICATE definition in the MICCertificates——–.h file

![AddSizeNumber](https://github.com/TelenorStartIoT/tutorials/blob/master/04-arduino-mkrnb1500-mqtt/assets/07-AddSizeNumber.jpg)

## Transform the PEM private key

To change the private key (privkey.pem) from PEM to DER format that is suitable in the MICCertificates——–.h is a little bit more tricky. Start by using this openssl command:

`openssl rsa -inform PEM -in privkey.pem -outform DER -out privkey.dat`

MAC/Linux:

Use this command to transform the binary file to the appropriate textual hex format:

`hexdump -e '16/1 "0x%02x, " "\n"' privkey.dat > privkey.hex`

Windows:

As the hexdump command is not native to Windows, we use a workaround by making a function that does the same thing, then call said function:
```
function Get-HexDump($path,$width=16, $bytes=-1) {
$OFS=""
Get-Content -Encoding byte $path -ReadCount $width `
-totalcount $bytes | Foreach-Object {
$byte = $_
if (($byte -eq 0).count -ne $width) {
$hex = $byte | Foreach-Object {
"0x" + ("{0:x}" -f $_).PadLeft(2,"0") + ", "}
"$hex"
} } }
set-content -path privkey.hex -value (get-hexdump privkey.dat)
```

You now have the format you need in the privkey.hex file but be aware that there could be some trailing 0x0 at the end of the file. If that is the case, they need to be removed when you copy this into the MICCertificates——–.h file.

To find the size (number of bytes) you can either count them manually 🙂 or use the ls -l command on the privkey.dat file:

```
> ls -l privkey.dat
```
![Length](https://github.com/TelenorStartIoT/tutorials/blob/master/04-arduino-mkrnb1500-mqtt/assets/Length.PNG)

In the above case, the size is 1194.

![StartFormat](https://github.com/TelenorStartIoT/tutorials/blob/master/04-arduino-mkrnb1500-mqtt/assets/08A-TransformPEMPrivateKey.jpg)
The start of the formatted privkey bytes in the MICCertificates——–.h file.

![EndFormat](https://github.com/TelenorStartIoT/tutorials/blob/master/04-arduino-mkrnb1500-mqtt/assets/08B-TransformPEMPrivateKey.jpg)
The end of the formatted privkey bytes in the MICCertificates——–.h file. Note where the size is placed and that trailing 0x0 has been removed.


### Run the example program

Compile and run the example code on the MKR1500 device by clicking on the upload arrow symbol or choosing “Sketch->Upload..” from the menu. Open the Serial Monitor (Tools->Serial Monitor…) and see the log output from your program.

![RunExampleProgram](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/17-run-program.png)
![RunExampleProgram2](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/18-run-program2.png)

### See your data displayed in MIC

Open the MIC dashboard and see your data displayed in MIC.

![MICDashBoard](https://github.com/TelenorStartIoT/tutorials/blob/master/03-arduino-mkrnb1500-coap/assets/19-example-dashboard.png)

## Happy hacking!
This concludes the Get started with the Arduino dev kit tutorial. Your next step could be to connect the supplied DHT11 sensor to the Arduino dev kit and to modify the “dummy” payload string with values from the DHT11 sensor.

A god starting point would be to find out how to use the DHT library code supplied here:

https://github.com/winlinvip/SimpleDHT

Happy hacking!











# Connecting sensors for Fipy

> Works with NB-IoT or LTE-M

This tutorial gives brief instructions on how to connect DHT11 temperature and humidity sensor to fipy device

You will learn how to:

   * How to connect sensors to fipy device
   * Programing device to measure temperature and humidity
   

## 1. How to connect sensors to fipy device

Hope you have successfully connected your fipy device and tried the example program in the previous tutorial.

This chapter will show you how to connect sensors DHT11 temperature and humidty sensors to your fipy device.

### 1.1 Attaching the sensor to your fipy device.

 You can see 'Three Jumper wires' with different colours have been attached to sensor.
 
 ![Sensor](https://github.com/TelenorStartIoT/tutorials/tree/master/14-Connecting-sensors-fipy/assets/00-DHT11_FipySensor.jpg)

- Bydefault one end has been attached to sensor and you need to attach the other end to your fipy device.
- To do so use additional **'M/M Jumpwires'** to connect sensor to your device.

![Jumper_wire](https://github.com/TelenorStartIoT/tutorials/tree/master/14-Connecting-sensors-fipy/assets/01-MTM-Jumperwire.jpg)

- To connect the pins check below image.
  ![Sensor_pin](https://github.com/TelenorStartIoT/tutorials/tree/master/14-Connecting-sensors-fipy/assets/02-Sensor_pin.jpg)


Once you connected sensor to your device. It should look like below.
![Connected_device](https://github.com/TelenorStartIoT/tutorials/tree/master/14-Connecting-sensors-fipy/assets/)


**Important:**
You should connect correct Pin from sensor to the respective pins to your device, or else it will damage the sensor and board.


## 2 Programing device to measure temperature and humidity

Now it's a time to start programming the dev-kit to measure temperature and humidity.

### 2.1 Download Example Code

You can download the example code from here: (addlink to program)

You should choose the "Download ZIP" option in the "Clone or Download" button pop-up. This will download a ZIP archive with the example code.

![Download ZIP](https://github.com/TelenorStartIoT/tutorials/blob/master/01-fipy-udp/assets/14-download-zip.png)

### 2.2 Unzip the Example Code and Open It In VSCode

Unzip the example code. How to do this varies depending on your computer system. Most systems will unzip it if you double click on the zip file.

Open the folder using the "File > Open Folder" ("File > Open" on MacOS) option in VSCode.

![Open project in VSCode](https://github.com/TelenorStartIoT/tutorials/blob/master/01-fipy-udp/assets/15-open-project.png)

![Open project in VSCode](https://github.com/TelenorStartIoT/tutorials/blob/master/01-fipy-udp/assets/16-open-project.png)

### 2.3 Run the Program

Connect the FiPy that is mounted on the expansion board to your computer (if not already). Make sure that the SIM and LTE antenna is connected! The Pymakr plugin in VSCode will automatically detect the dev-kit.

To upload and run the program on your FiPy, simply click the "Upload" button located at the bottom bar. This will first upload the code, then it will reset your FiPy and run the uploaded code.

### 5.5 Check the Output From the Program

If everything goes well you should see output from the program in the Pycom Console window in VSCode. The image shows what it could look like (the output from the program might be different).

![Program output](addlink)

#### 5.5.1 Unable to run program

If you are not able to run the example code and see below error in your console. 

![port error](https://github.com/TelenorStartIoT/tutorials/blob/master/01-fipy-udp/assets/17-fipy-error.jpg)

Follow below steps
- Disconnect your device.
- Connect the pin '3V3' and 'P12'. 
- Monitor LED light,the heartbeat LED will begin flashing orange slowly. If after 3 seconds the pin is still held high, the LED will start blinking faster. At this point release the wire from pin 'P12'.
- Connect your device using USB and check if you were able to run the program.

If everything goes well, you will able to see the output in console.


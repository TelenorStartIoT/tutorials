# Configure your Thingy:91 in MIC and flash with a pre-compiled .hex file

Start by downloading the nRF Connect software from Nordic Semiconductors here: https://www.nordicsemi.com/Software-and-tools/Development-Tools/nRF-Connect-for-desktop

Open nRF Connect and press the install button to the right of the applications "LTE Link Monitor". 

![](https://github.com/TelenorStartIoT/tutorials/blob/master/05-thingy-get-started/assets/1.1-nrf-link-monitor.png)

## Configuring your Thingy:91 in MIC

### 1. Retreiving the IMSI and IMEI numbers
For your Thingy:91 to send data to the Managed IoT Cloud platform, it needs to be registered and configured correctly. To do this you need the IMSI number of the SIM card and the IMEI number of the Thingy:91. Both these numbers can be retreived by sending AT commands to the Thingy. In the "LTE Link Monitor" application there is a terminal that allows you to send AT commands to your Thingy:91. Plug in your Thingy and open the "LTE Link Monitor", make sure the Thingy is turned on. 

Choose your device in the drop-down menu in the top left corner. 

![](https://github.com/TelenorStartIoT/tutorials/blob/master/05-thingy-get-started/assets/1.2-select-device.png)

You should then be able to see the datastream in the terminal window. In the writing field below the terminal you can write AT commands to the Thingy. AT+CIMI will give you the IMSI number of the SIM card, and AT+CGSN will return the IMEI number of the Thingy. You should note these numbers somewhere as you will need them in the next step. 

![](https://github.com/TelenorStartIoT/tutorials/blob/master/05-thingy-get-started/assets/1.3-at-commands.png)

### 2. Setting up device in MIC
2.1 Log in to MIC with your username and password.

- If you do not have a user yet you can sign up using this link: https://demonorway.mic.telenorconnexion.com/ 

    Click on the "Sign Up" button in the upper right corner and follow the instructions in order to sign up. It is important that you fill out at least the six first fields in addition to company. This is a prerequisite for being verified as a user.

    **Note:** You should be aware that the signup is two-phased, and after you have confirmed your email address it may take some time before your user account is activated. This is a manual verification process and may take up to 24 hours outside normal business hours. Once your account is ready for use you will receive an email stating that your account has been activated. 

2.2 Create a *Thing Type*

Once logged in to the platform, the first thing you have to do is make a new *Thing Type*. You do so by pressing the blue button "New Thing Type" in the top left corner.  
   * A *Thing Type* is a method of organising multiple "Things" that report on the same data resources. 
   * A *Thing* must always belong to a *Thing Type*, this is why we have to make this before registering a "Thing".


![](https://github.com/TelenorStartIoT/tutorials/blob/master/05-thingy-get-started/assets/1.4-new-thing-type.PNG)

Give your *Thing Type* a label and a description of your choice. 

In the "Uplink Transform" add the following code:

```javascript
var resources = payload.toString("utf8").split(",");
var MESSAGE_TYPE = resources.shift();

var params = [
   // Modem message
   [
      { name: 'uptime', type: 'int' },
      { name: 'current_band', type: 'int' },
      { name: 'area_code', type: 'int' },
      { name: 'current_operator', type: 'string' },
      { name: 'mcc', type: 'int' },
      { name: 'mnc', type: 'int' },
      { name: 'cellid', type: 'string' },
      { name: 'ip_address', type: 'string' },
      { name: 'modem_fw', type: 'string' },
      { name: 'battery', type: 'int' },
      { name: 'imei', type: 'int' }
   ],
   // Sensors message
   [
      { name: 'uptime', type: 'int' },
      { name: 'temperature_float', type: 'float' },
      { name: 'humidity_float', type: 'float' },
      { name: 'pressure_float', type: 'float' },
      { name: 'air_quality_float', type: 'float' },
      { name: 'red', type: 'int' },
      { name: 'green', type: 'int' },
      { name: 'blue', type: 'int' },
      { name: 'ir', type: 'int' },
      { name: 'x', type: 'int' },
      { name: 'y', type: 'int' },
      { name: 'z', type: 'int' }
   ]
];

return params[MESSAGE_TYPE].reduce(function (a, b, i) {
   var val = resources[i];
   if (b.type == 'int') val = parseInt(val);
   if (b.type == 'float') val = parseFloat(val);
   a[b.name] = val;
   return a;
}, {});
```

In the same way, add this piece of code in the "Downlink Transform" to be able to send messages back to your *Thing*:

```Javascript
return {
   transport: "coap-push",
   port: 5683,
   coapPath: "/",
   payload: [state]
}
```

- This code is just one example of what the uplink transform can look like. It is tailored to how the current code on the Thingy:91 sends its data and makes it readable to MIC. This will separate each property of the object into its separate "parts". For each "part" a resource in MIC will be created.

    Do not worry about the details for now, this was just for your information. It is possible to create uplink transformations for payloads formatted in basically any format (hex, binary, text, JSON, etc.). 

    The uplink transform is just a snippet of JavaScript code that MIC will use when doing transformations on your payload, and is generally used to unpack compressed payloads into understandable resources.

![](https://github.com/TelenorStartIoT/tutorials/blob/master/05-thingy-get-started/assets/1.5-thing-type.PNG)

Now that you have your *Thing Type* you can create your *Thing*. 

2.3 Create your *Thing*

Your *Thing Types* will be visible to you in the panel on the left hand side of the window. When creating a new *Thing* it will automatically belong to the selected *Thing Type*.

![](https://github.com/TelenorStartIoT/tutorials/blob/master/05-thingy-get-started/assets/1.6-new-thing.PNG)

To add a new Thing, click on the **"+ THINGS"** button in the top right corner of your screen. A pop-up window will appear. 
   * De-select the **"Create batch"** slider in the pop-up window.
   * You must then add a **"Thing Name"**, a **"Description"** and select your **"Domain"** from the drop-down menu.
   * Choose **"Telenor IoT Gateway (NB-IoT/LTE-M)"** as the network integration for your Thing. You will also have to add the IMSI and IMEI numbers that we got earlier.

![](https://github.com/TelenorStartIoT/tutorials/blob/master/05-thingy-get-started/assets/1.7-thing.PNG)

You are now finished registering your thing in MIC, and should be able to see some data coming in. The next step for you is to create widgets displaying your data and tailor your dashboard.

### 3 View and edit your dashboard
If you click on the name of your *Thing* in the list you will open the dashboard for your Thing. This dashboard will be mainly empty until the first payload from your dev-kit arrives. The dashboard is configurable and you can add widgets that represents values sent from your dev-kit. The diferent values are called resources. For example: Temperature can be a resource. 

To edit your dashboard you have to press the *move* button in the top right corner. This will enable edit mode where you can move things around and create new widgets. 

![](https://github.com/TelenorStartIoT/tutorials/blob/master/05-thingy-get-started/assets/1.8-move.PNG)

Once you are in edit mode you can create new widgets and tailor the dashboard to your use-case. Press the button **"+Widgets"** to create a new widget. 

![](https://github.com/TelenorStartIoT/tutorials/blob/master/05-thingy-get-started/assets/1.9-new-widget.png)

In the drop down menu labeled "Resources" you will find a drop-down menu displaying the different data your *Thing* is sending. These can be viewed in a number of different widgets and I reccomend you to play around a bit with them to find an optimal solution for your use-case. Also, **remember to press save** when you have done new changes, otherwise the edits will be lost. 

![](https://github.com/TelenorStartIoT/tutorials/blob/master/05-thingy-get-started/assets/1.10-widget.PNG)

![](https://github.com/TelenorStartIoT/tutorials/blob/master/05-thingy-get-started/assets/1.11-save.png)

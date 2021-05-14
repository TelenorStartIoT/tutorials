# Managed IoT Cloud - MQTT API

Telenor Managed IoT Cloud has several API's to query data and interact with the platform. The Thing subscription MQTT API is used to connect to the MQTT broker that Managed IoT Cloud maintains and enables you to communicate live (both ways) with your IoT devices. The Thing Subscription MQTT API can be used to show a live feed of IoT data, but can also be used to send commands downlink to IoT devices. In this document you will:

  * Learn to create a Managed IoT Cloud MQTT client
  * Use the MQTT client to subscribe to topics and get live data
  * Create a gauge that is updated live

**NB! This code example is for prototyping only. Not for deployment in production.**

## 1. Setup HTML Page and Fetch Necessary Libraries

The gauge chart will be created in a static HTML page that you can open in any web browser of your choice. We will use the C3.js JavaScript library for creating the gauge chart and a [JavaScript MIC SDK](https://github.com/TelenorStartIoT/mic-sdk-js). The MIC SDK will do most of the "heavy work" for us so that we can focus on creating the application.

Start by creating an "index.html" file with the following contents:

```html
<html>
  <head>
    <title>My Awesome Live Chart</title>
    <!-- Load c3.css -->
    <link href="https://cdnjs.cloudflare.com/ajax/libs/c3/0.6.13/c3.min.css" rel="stylesheet">
  </head>
  <body>
    <!-- Our chart -->
    <div id="chart"></div>

    <!-- An MQTT message log (for debugging) -->
    <pre id="log"></pre>

    <!-- Load d3.js and c3.js -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/d3/5.9.1/d3.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/c3/0.6.13/c3.min.js"></script>
    <!-- Load MIC SDK -->
    <script src="https://unpkg.com/mic-sdk-js@latest/dist/mic-sdk-js.min.js"></script>
    <script>
      // Your code goes here!
    </script>
  </body>
</html>
```

If you're running on Windows make sure that the file extension is in fact ".html". You may have to enable file extensions by clicking "View > File name extensions" in the file browser.

### 1.1 So, what's included?

In the head-section we include the stylesheet for the C3.js library. Then, in the top of the body-section we create a div-element with the ID "chart". This is where the chart will finally go. We also create another pre-element where MQTT messages will be logged. After that we include three other JavaScript libraries; D3.js (which is a dependency of C3.js), C3.js and the MIC SDK. At the end there is a script-block where you will continue to write your code.

## 2. Create an Empty Gauge Chart

Add the following code within the script-block (where it says "Your code goes here!"):

```js
// Create the gauge chart
var chart = c3.generate({
  bindto: '#chart',
  data: {
    type: 'gauge',
    columns: [
      ['temperature', 0.0]
    ]
  },
  gauge: {
    min: -50,
    max: 50,
    label: {
      format: function (value) {
        return value + '°C';
      }
    }
  }
});
```

Here we generate a new C3 gauge chart and save it to the "chart" variable. The chart is bound to the div with the ID "chart" that we created earlier in the document. We add a single column called "temperature" with the initial value 0.0. The gauge has a minimum value of -50 and a maximum value of 50. The label is formatted by appending a Celsius unit string to the gauge value.

Save the file and open it in a web browser. You should see an empty chart:

![Empty C3.js Gauge Chart](https://github.com/TelenorStartIoT/tutorials/blob/master/13-mqtt-api/assets/empty-chart.png "Empty C3.js Gauge Chart")

Not very interesting. Let's get some live data!

## 3. Create an MQTT Client Using the MIC SDK

Continue by adding the following code below the previous code:

```js
// Init with username and password
MIC.init({
  username: '<MIC username>',
  password: '<MIC password>'
}).then(() => {

  // Create a new MQTT client
  MIC.mqtt()
    .then((client) => {
      // Do something with the MQTT client here!
    });
});
```

### 3.1 Let's take a closer look at what's happening here

```js
MIC.init({
  username: '<MIC username>',
  password: '<MIC password>'
})
```

Here we initialize the MIC SDK by providing the MIC username and password. You must replace this with the real username and password of your MIC user that you previously created.

```js
// Create a new MQTT client
MIC.mqtt()
  .then((client) => {
    // Do something with the MQTT client here!
  });
```

Here we create a new MQTT client that is automatically configured to connect to the Managed IoT Client MQTT broker. The client that is returned here is a wrapper for the [mqtt.Client()](https://github.com/mqttjs/MQTT.js/blob/master/README.md#client) and can be treated as such.

If you save the file and refresh the webpage nothing happens, this is because we actually don't do anything with the MQTT client yet.

## 4. Connect the MQTT Client and Subscribe to a Topic

Add the following code after the MQTT client is created (where it says "Do something with the MQTT client here!"):

```js
// Event listener when the client connects
client.on('connect', function () {
  // Immediately subscribe to a topic
  client.subscribe('thing-update/StartIoT/dust/00001347');

  // Log event
  document.getElementById('log').innerHTML += 'Subscribed!\n\n';
});
```

Here we attach an event listener to the MQTT client for the "connect" event. The function is a callback and will be invoked once the client connects to the MQTT broker. The function will immediately subscribe to a MQTT topic.

You must now find the correct MQTT topic of you Thing. The topic will always begin with `thing-update/` if using the MIC SDK. What comes after is every "domain" that the Thing is placed in. If you're enrolled in the Telenor Start IoT program you must add the `StartIoT` domain. The last part of the MQTT topic is the Thing Name. You can find the Thing Name of your Thing in the list in AppBoard (the dashboard). A Thing Name usually takes the form of an increasing number, such as "00001337". Alternatively, you can use the wildcard character "#" to select all Things under a certain domain.

E.g. to subscribe to a topic for all Things under the domain called "dust" in the "StartIoT" domain, use the topic: "thing-update/StartIoT/dust/#"

Save the file and reload the webpage. After a while you will see a "Subscribed!" message in the log:

![Subscribe Successful](https://github.com/TelenorStartIoT/tutorials/blob/master/13-mqtt-api/assets/subscribed.png "Subscribe Successful")

If you're not seeing the "Subscribed!" message you must go back and check that you are using the correct MQTT topic.

## 5. Update the Gauge Chart with Live Data

Continue by adding the "message" event listener below the "connect" event listener:

```js
// Event listener for when a message is received
client.on('message', function (topic, message) {
  try {
    var json = JSON.parse(message.toString());

    // Get the correct resource from the message
    var value = json.state.reported.temperature;
    
    // Update gauge chart
    chart.load({
      columns: [['temperature', value]]
    });

    // Log message
    document.getElementById('log').innerHTML += message.toString() + '\n\n';
  } catch (e) {
    // Log error
    document.getElementById('log').innerHTML += 'Failed to parse message: ' + e.message;
  }
});
```

When we attach the "message" event listener to the MQTT client, the callback function will be invoked every time a new message is sent over the MQTT topic that you are subscribed to. The message must be parsed as JSON and the correct resource property must be selected. If you device is sending data to MIC and the uplink transform is creating a resource called "temperature", you don't need to change anything. We then update the gauge chart with the new data and log the message.

Save the file and reload the webpage. After a while you will see the gauge updated live as new messages are received and the log will show the message payloads:

![Gauge Chart Updated Live](https://github.com/TelenorStartIoT/tutorials/blob/master/13-mqtt-api/assets/live.png "Gauge Chart Updated Live")

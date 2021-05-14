# Managed IoT Cloud - Elasticsearch API

The Managed IoT Cloud App Board is great for creating dashboards to visualize data that your devices send to the cloud. However, if you want to customize the look and feel of your graphs and charts you must create them yourself using the various API's that MIC offers. In this document you will:

  * Use the MIC Elasticsearch API to query historical data from a device
  * Plot the data in a timeseries chart
  * Display it in a static HTML page  
 
**NB! This code example is for prototyping only. Not for deployment in production.** 

## 1. Setup HTML Page and Fetch Necessary Libraries

The timeseries chart will be created in a static HTML page that you can open in any web browser of your choice. We will use the C3.js JavaScript library for creating the timeseries chart and a [JavaScript MIC SDK](https://github.com/TelenorStartIoT/mic-sdk-js). The MIC SDK will do most of the "heavy work" for us so that we can focus on creating the application.

Start by creating an "index.html" file with the following contents:

```html
<html>
  <head>
    <title>My Awesome Chart</title>
    <!-- Load c3.css -->
    <link href="https://cdnjs.cloudflare.com/ajax/libs/c3/0.6.13/c3.min.css" rel="stylesheet">
  </head>
  <body>
    <!-- Our chart -->
    <div id="chart"></div>

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

In the head-section we include the stylesheet for the C3.js library. Then, in the top of the body-section we create a div-element with the ID "chart". This is where the chart will finally go. After that we include three other JavaScript libraries; D3.js (which is a dependency of C3.js), C3.js and the MIC SDK. At the end there is a script-block where you will continue to write your code.

## 2. Create an Empty Timeseries Chart

Add the following code within the script-block (where it says "Your code goes here!"):

```js
// Create the chart
var chart = c3.generate({
  bindto: '#chart',
  data: {
    x: 'x',
    columns: [
      ['x'],
      ['y']
    ]
  },
  axis: {
    x: {
      type: 'timeseries',
      tick: {
        format: '%Y-%m-%d'
      }
    }
  }
});
```

Here we generate a new C3 chart and save it to the "chart" variable. The chart is bound to the div with the ID "chart" that we created earlier in the document. We then add two columns; "x" and "y". The "x" column will be the X-axis and will contain timestamps. The "y" column will be the Y-axis and contain device data that we will get from MIC. We also need to tell C3 that the "x" column is in fact the X-axis. At the end we define the chart type as a "timeseries" and tell C3 to format each "tick" on the X-axis as "Year-Month-Day".

Save the file and open it in a web browser. You should see an empty chart:

![Empty C3.js Chart](https://github.com/TelenorStartIoT/tutorials/blob/master/12-elasticsearch-api/assets/empty-chart.png "Empty C3.js Chart")

Not very interesting. Let's get some data!

## 3. Run an Elasticsearch Query Using the MIC SDK

Continue by adding the following code below the previous code:

```js
// Init with username and password
MIC.init({
  username: '<MIC username>',
  password: '<MIC password>'
}).then(() => {

  // Do an Elasticsearch query to get Thing data
  MIC.elasticsearch({
    query: {
      size: 1000, // Set a limit of 1000 "hits"
      query: {
        bool: {
          filter: [
            {
              term: {
                thingName: '<MIC Thing Name>'
              }
            }
          ]
        }
      }
    }
  })
    .then((result) => {
      // Do something with the result here!
    });
});
```

### 3.1 That's a lot of code! Let's dissect it:

```js
MIC.init({
  username: '<MIC username>',
  password: '<MIC password>'
})
```

Here we initialize the MIC SDK by providing the MIC username and password. You must replace this with the real username and password of your MIC user that you previously created.

```js
// Do an Elasticsearch query to get Thing data
MIC.elasticsearch({
  query: {
    size: 1000, // Set a limit of 1000 "hits"
    query: {
      bool: {
        filter: [
          {
            term: {
              thingName: '<MIC Thing Name>'
            }
          }
        ]
      }
    }
  }
})
```

Here we do the Elasticsearch query itself. An upper limit of 1000 hits is set, but you can change this. We then apply a boolean filter where we specify that the "term" must be a "thingName" equal to our Thing Name. Here you must change the value to the Thing you want to query. You can find the Thing Name of your Thing in the list in AppBoard (the dashboard). A Thing Name usually takes the form of an increasing number, such as "00001337".

If you save the file and refresh the webpage nothing happens, this is because we actually don't do anything with the data yet.

## 4. Add Data to the Chart

Add the following code after the Elasticsearch query (where it says "Do something with the result here!"):

```js
// Create new x/y columns
var x = ['x'];
var y = ['y'];

// The resource representing the Y-axis value
var resource = 'temperature';

// Loop through each "hit" in the Elasticsearch query result
for (let i in result.hits.hits) {
  var currentHit = result.hits.hits[i];

  // Pick out timestamp and value from the "hit"
  var timestamp = currentHit._source.timestamp || null;
  var value = currentHit._source.state[resource] || null;

  // Add to new columns
  x.push(timestamp);
  y.push(value);
}

// Swap out old data with new data in our chart
chart.load({
  unload: true,
  columns: [x, y]
});
```

The code should be self documenting but you must replace the "resource" varible with the resource name of the resource you want to query. If you device is sending data to MIC and the uplink transform is creating a resource called "temperature", you don't need to change anything.

Save the file and reload the webpage. After a while you will see the chart get populated with data:

![Populated C3.js Chart](https://github.com/TelenorStartIoT/tutorials/blob/master/12-elasticsearch-api/assets/populated-chart.png "Populated C3.js Chart")

That's it! Go and try different Elasticsearch queries and create your own unique charts!

### 4.1 Some Inspiration

![Inspiration](https://github.com/TelenorStartIoT/tutorials/blob/master/12-elasticsearch-api/assets/inspiration.png "Inspiration")

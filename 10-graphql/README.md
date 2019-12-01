# Using the Managed IoT Cloud GraphQL API to Interact with the Platform

In addition to a RESTful API, Managed IoT Cloud also offers a GraphQL endpoint to interact with the API. GraphQL offloads much of the potential querying a developer has to make with a "traditional" RESTful API. Instead, a Graph query is sent to a single GraphQL endpoint and the desired data can be pinpointed by the server, drastically reducing the amount of network roundtrips a client has to make. In this document you will:

  * Learn to use the GraphiQL playground
  * Use the GraphQL API to query Things
  * Display a list of Things in a static HTML page

### Contents

  1. Use the MIC GraphiQL Playground to Test GraphQL Queries
  2. Setup HTML Page and Run a GraphQL Query
  3. Run a GraphQL Query Using the MIC SDK

# 1. Use the MIC GraphiQL Playground to Test GraphQL Queries

In order to explore the "graph" that the MIC GraphQL API exposes, you can use the GraphiQL playground to test and explore available queries. Access the playground here:

[https://demonorway.mic.telenorconnexion.com/graphiql](https://demonorway.mic.telenorconnexion.com/graphiql)

You must login with your MIC accound that you have previously aquired.

### 1.1 Browse Available Queries

In the top right corner in the GraphiQL playground click the "Docs" button to open the "Documentation Explorer". You will see two root types; query and mutation. For now, click on the query type.

![GraphiQL Query Explorer](https://github.com/TelenorStartIoT/tutorials/blob/master/10-graphql/assets/query-explorer.png "GraphiQL Query Explorer")

You now see a list of available queries (or "nodes") in the GraphQL API.

### Try Some Queries

Now let's try some queries. In the left side pane, insert the following query:

```
{
  allThings {
    things {
      label
      description
      thingName
      imei
      imsi
    }
  }
}
```

Here we query the "allThings" node and tell it to return a list of Things and include the label, description, thingName, imei and imsi of each Thing. Click the "play" button or CTRL+ENTER to run the query. The result is displayed in the right pane.

![GraphiQL Query Result](https://github.com/TelenorStartIoT/tutorials/blob/master/10-graphql/assets/query-result.png "GraphiQL Query Result")

By using the "Documentation Explorer" you can browse available queries and immediately try them.

When you're writing queries you can click SHIFT+SPACE to get a list of available fields in the current node. This is very useful and saves you time from browsing the "Documentation Explorer".

![GraphiQL Query Helper](https://github.com/TelenorStartIoT/tutorials/blob/master/10-graphql/assets/query-helper.png "GraphiQL Query Helper")

## 2. Setup HTML Page and Run a GraphQL Query

You will now create a static HTML page that uses the [JavaScript MIC SDK](hhttps://github.com/TelenorStartIoT/mic-sdk-js) and run a GraphQL query. The result will be displayed in plain text on the web page.

Create an "index.html" file with the following contents:

```html
<html>
  <head>
    <title>My MIC GraphQL Application</title>
  </head>
  <body>
    <!-- The GraphQL output element -->
    <pre id="output"></pre>

    <!-- Load MIC SDK -->
    <script src="https://unpkg.com/mic-sdk-js@latest/dist/mic-sdk-js.min.js"></script>
    <script>
      // Your code goes here!
    </script>
  </body>
</html>
```

If you're running on Windows make sure that the file extension is in fact ".html". You may have to enable file extensions by clicking "View > File name extensions" in the file browser.

### 2.1 So, what's included?

A pre-element with the ID "output" is created in the body-section. This element will contain the result from the GraphQL query. Then, the MIC SDK library is loaded. At the end there is a script-block where you will continue to write your code.

## 3. Run a GraphQL Query Using the MIC SDK

Add the following code within the script-block (where it says "Your code goes here!"):

```js
// Init with username and password
MIC.init({
  username: '<MIC username>',
  password: '<MIC password>'
}).then(() => {

  // Run a GraphQL query
  MIC.graphql({
    query: `
      {
        allThings {
          things {
            label
            description
            thingName
            imei
            imsi
          }
        }
      }
    `
  })
    .then((result) => {
      // Convert the result into a string
      var str = JSON.stringify(result, null, 2);

      // Insert the string into the <pre>-element
      document.getElementById('output').innerHTML = str;
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
// Run a GraphQL query
MIC.graphql({
  query: `
    {
      allThings {
        things {
          label
          description
          thingName
          imei
          imsi
        }
      }
    }
  `
})
```

Here we do the GraphQL query itself. As you can see we use the same query that we used in the GraphiQL playground. You can insert any query here that works in the GraphiQL playground.

```js
// Convert the result into a string
var str = JSON.stringify(result, null, 2);

// Insert the string into the <pre>-element
document.getElementById('output').innerHTML = str;
```

At the end we take the result and convert it into a string. The string is then inserted into the pre-element that was previously created in the body-section.

Save the file and open it in a web browser. After a while you will see the result of the GraphQL query displayed:

![HTML Result](https://github.com/TelenorStartIoT/tutorials/blob/master/10-graphql/assets/html-result.png "HTML Result")

## 4. Add Styles to the List

Displaying the "raw" output of the GraphQL query is not very pretty. We will now add some CSS styles and change the output into a list of Things.

Change the pre-element to a div-element:

```html
<!-- Change this: -->
<pre id="output"></pre>

<!-- To this: -->
<div id="output"></div>
```

Continue by adding the following CSS styles to the head-section:

```html
<style>
* {
  margin: 0;
  padding: 0;
}
.thing {
  margin: 10px;
  color: #FFF;
  background: #009cff;
  border: 5px solid #0075bf;
  border-radius: 5px;
  font-family: Arial, Helvetica, Verdana;
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
}
.thing > div {
  width: 50%;
  padding: 15px;
}
.thing > div:last-child {
  background: #0075bf;
}
.thing b {
  float: right;
}
</style>
```

Finally, change the code where the result from the GraphQL query is parsed as a string and inserted into the pre-element to this:

```js
.then((result) => {
  // Get the list of Things from the GraphQL result
  var things = result.data.allThings.things;

  // Clear the list
  document.getElementById('output').innerHTML = '';

  // Loop through each Thing in the GraphQL result list
  for (let i in things) {
    var thing = things[i];

    // Create HTML for a singe Thing
    var html = `
      <div class="thing">
        <div>
          <h1>${thing.label}</h1>
          <h2>${thing.description}</h2>
        </div>
        <div>
          <div>Thing Name: <b>${thing.thingName}</b></div>
          <div>IMSI: <b>${thing.imsi}</b></div>
          <div>IMEI: <b>${thing.imei}</b></div>
        </div>
      </div>
    `;

    // Insert the Thing into the list
    document.getElementById('output').innerHTML += html;
});
```

Save the file and reload the webpage. You will now see a much nicer list of Things:

![Nice List](https://github.com/TelenorStartIoT/tutorials/blob/master/10-graphql/assets/nice-list.png "Nice List")

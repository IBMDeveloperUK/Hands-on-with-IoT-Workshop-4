# Hands on with IoT
Part 4

## In this workshop we will...

Create a web app to interact with a datastore of historical IoT sensor data (from an elevator)

## You will need

1. Either a laptop with Node-RED installed **OR** An IBM Cloud account with the Node-RED starter, and a computer to access it with
2. A web browser to view the app that we're building in

## Setting up the web app in Node-RED

1. Head to your Node-RED instance (either http://localhost:1880 or your Node-RED started URL) and create a new flow.
2. Drag a HTTP input node from the panel on the left to somewhere on your canvas (the blank white space in your Node-RED window). You can filter the available nodes by using the input at the top left of the window
3. Double click the HTTP node to configure it:
    - Enter `/elevators` in the `URL` field
    - Enter a name for the node if you wish, but this is not required for the workshop
4. Drag three `template` nodes onto the canvas.

*NB: Don't forget to periodically deploy your changes with the red `Deploy` button in the top right corner to save your progress.*

5. Connect the three nodes to each other, and connect the output for the `HTTP` node to the input of the leftmost template node
6. Double click on the leftmost `template` node and configure it as follows:
    - Set the `Name` value to 'Styles'
    - Change the `Set property` value to `payload.styles`
    - Select `CSS` from the `Syntax Highlight` dropdown
    - Remove all of the content from the `Template` text area
    - Click `Done` to save your changes
7. Double click on the middle `template` node and configure it as follows:
    - Set the `Name` value to 'JavaScript'
    - Change the `Set property` value to `payload.script`
    - Select `JavaScript` from the `Syntax Highlight` dropdown
    - Remove all of the content from the `Template` text area
    - Click `Done` to save your changes
8. Click the final `template` node and configure it as follows, but do not click `Done`:
    - Set the `Name` value to 'HTML'
9. Before you close the tab, copy and paste the following code into the `Template` text area, and then click `Done` to save your changes.

```
<!DOCTYPE html>
<html>
    <head>
        <meta name="viewport" content="initial-scale=1.0, user-scalable=yes" />
        <link href="https://fonts.googleapis.com/css?family=IBM+Plex+Sans:300,400,500,600,700" rel="stylesheet">
        <link href="https://unpkg.com/carbon-components/css/carbon-components.min.css" rel="stylesheet">
        <script src="https://unpkg.com/carbon-components/scripts/carbon-components.min.js"></script>
        <style>
        {{{payload.styles}}}
        </style>
    </head>
    <body>
        <script>{{{payload.script}}}</script>
    </body>
</html>
```

The `{{{}}}` is handlebars syntax which will load the content from our styles and script template nodes into our HTML. All of our code could be written in one template Node, but it's often helpful to seperate out CSS, JavaScript, and HTML from one another for readability.

10. Finally, drag an HTTP output node onto you canvas and connect it to the HTML template node.
11. We now have a single page web app that can be delivered to a browsers. Click the `Deploy` button and head to either `http://localhost:1880/elevators` (for locally running Node-RED) or `https://YOUR_IBM_CLOUD_NODE_RED_STARTER_URK.bluemix.net/elevators` to view the web page we just created.
12. If all has gone well, you should now see an empty web page.

## Styles and additional Markup

Every web app needs styles and markup to function well, so we're going to add some now.

1. Head back to your Node-RED flow and double click the `Styles` template node that we created in the previous steps (step 6)
2. Copy and paste the following code into the Template text area, and then click 'Done' to save your changes.

```CSS
*[data-active="false"]{
    display: none !important;
}


html, body{
    padding : 0;
    margin: 0;
    width: 100%;
    height: 100%;
}

body{
    display: flex;
    flex-direction: column;
    background: rgb(245,245,245);
    font-family: 'IBM Plex Sans', sans-serif;
    box-sizing: border-box;
    padding-top: 50px;
}

header{
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 50px;
    background: #466bb0;
    color: white;
    justify-content: center;
    align-items: center;
    display: flex;
    font-weight: 800;
    font-size: 1em;
}

header #back{
    position: absolute;
    left: 1em;
    font-weight: 200;
    display: flex;
    align-items: center;
    justify-content: center;
    top: 0;
    height: 100%;
    cursor: pointer;
}

h1, h2, h3, h4, h5, h6{
    margin-top: 0;
    margin-bottom: 0.5em;
    font-weight: 600;
}

p{
    margin: 0;
}

#elevators{
    display: flex;
    width: 100%;
    height: 100%;
    margin: 0;
    flex-direction: column;
    align-items: flex-start;
    justify-content: flex-start;
    box-sizing: border-box;
    padding: 0;
    list-style-type: none;
}

#elevators li{
    padding: 1em;
    border-bottom: 1px solid black;
    width: 100%;
    cursor: pointer;
    box-sizing: border-box;
    background: white;
}

#info{
    display: flex;
    flex-direction: column;
    box-sizing: border-box;
    padding: 1em;
}

#info #wrapper{
    display: flex;
    width: 100%;
}

#info #wrapper #vis{
    min-width: 300px;
    height: 300px;
    display: flex;
    justify-content: center;
    align-items: center;
    background: #939393;
    color: white;
}

#info #wrapper #data{
    width: 100%;
    box-sizing: border-box;
    margin: 0;
    padding: 0 1em;
    max-width: 500px;
}

#info #wrapper #data #timeline{
    width: 100%;
    height: 40px;
    background: #616060;
    margin-bottom: 1em;
    border-radius: 3px;
    box-sizing: border-box;
}

#info #wrapper #data #timeline #slider{
    width: 15px;
    height: 100%;
    background: #ff6d6d;
    box-shadow: 0 1px 1px;
    transform: scale(1.2);
    transform-origin: center;
    cursor: pointer;
}

#info #wrapper #data input[type="range"]{
    width: 100%;
}

#info #wrapper #data li{
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
}
```

3. We now have lovely styles for our web app, but we also need some HTML that our JabvScript is going to interact with. Double click on the `HTML` template node in your flow.
4. Copy and paste the following markup after the `<body>` tag and before the `<script>` tag.

```HTML
<header>
    <div id="back" data-active="false">&lt;</div>
    Elevator Data
</header>

<ul id="elevators" data-active="true">
    
</ul>

<div id="info" data-active="false">
    <h1 id="title"></h1>
    <div id="wrapper">
        <div id="vis">
            <h1>Idle</h1>
        </div>
        <div id="data"></div>
    </div>
    
</div>

<div class="bx--loading-overlay" data-active="true">
    <div data-loading class="bx--loading">
        <svg class="bx--loading__svg" viewBox="-75 -75 150 150">
            <title>Loading</title>
            <circle cx="0" cy="0" r="37.5" />
        </svg>
    </div>
</div>
```
5. Click 'Done' to save your changes and then `Deploy`. If you view your web page again, you should now see a title bar and a spinning loading icon. Nothing is broken or loading at this point, this is expected behaviour.

We now have all of the CSS and markup that we need to start writing the logic of our application.

## Using JavaScript to get information about available devices.

First, we're going to use AJAX (asyncronous JavaScript and XML) to get information from a simple API about the elevators in our database, and then populate a list in our web app.

1. Head back to your Node-RED flow and double click the 'JavaScript' template node to start adding JavaScript to our web app.
2. Copy and paste the following code into the `Template` text area and then click `Done` to save your changes.

```JavaScript

(function(){
    
    // Block 1

    'use strict';
    
    const APIRoot = `https://sean-tracey-london-dev-node-red.eu-gb.mybluemix.net/elevators/api`;
    
    const elevatorHolder = document.querySelector('#elevators');
    const infoHolder = document.querySelector('#info');
    const back = document.querySelector('#back');
    const loading = document.querySelector('.bx--loading-overlay');
    
    let currentData;
    let currentRangeOffset = 0;
    let currentDataOffset = 0;

    // Block 2
    
}());
```
This contains the variables that we'll be using to track the state of our app as it's being used.

3. Now we're ready to start getting information from our database. We're not going to read information directly from the database, instead we're going to interact with an API that has been constructed for this workshop.
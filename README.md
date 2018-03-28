# Hands on with IoT
Part 4

## In this workshop we will...

Create a web app to interact with a datastore of historical IoT sensor data (from an elevator)

## You will need

1. Either a laptop with Node-RED installed **OR** An IBM Cloud account with the Node-RED starter, and a computer to access it with
2. A web browser to view the app that we're building in

**Creating a Node-RED app**

1. [Sign up for an account here](https://ibm.biz/BdZymc)
2. Verify your account by clicking on the link in the email sent to you
3. Log in to your IBM Cloud account
4. Click on "Catalog" on the top-right corner
5. Search and select "Node-RED Starter" 
6. Give a unique name to your app and click "Create"

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
9. Before you close the tab, copy and paste the following code into the `Template` text area (replacing what is already there), and then click `Done` to save your changes.

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

Copy and paste the following JavaScript just beneath the `// Block 2` statement

```JavaScript
fetch(`${APIRoot}/devices`)
    .then(res => {
        if(res.ok){
            return res.json();
        } else {
            throw res;
        }
    })
    .then(result => {
        console.log(result);

        // Code Block 3
        
    })
    .catch(err => {
        console.log('Fetch err:', err);
    })
;
```

4. Click `Done` to save your changes, and then click `Deploy`. 
5. Somewhere on your web page, right click and then select `Inspect`. Developer tools will open either to the side or bottom of your window. Click the console tab and reload your page. If all has gone well, you should see `{data: Array(10)}`. That's the data about our devices! If you want to look at the information contained within, you can click on the data to expand the view.
6. Now that we have information about our elevators, we want to display that in our page. We're going to use JavaScript to create some HTML that makes a list of devices that we can get information about. Head back to your Node-RED flow and double click the `Script` template node to edit our JavaScript once again.
7. Copy and paste the following code just beneath `// Code Block 3`, and then deploy your changes.

```JavaScript

const devices = result.data;

devices.forEach(device => {
    
    const li = document.createElement('li');
    li.textContent = device.deviceId;
    li.dataset.id = device.deviceId;

    // Code Block 4
    
    elevatorHolder.appendChild(li);
    
    loading.dataset.active = "false";

});
```

If you reload your web page, you should now see a list of elevators!

8. Now it's time to add functionality to the list. We're going to add some code that will let us select one of the available elevators, and go and get some data about its state over time. For this, we need to add an events listener to each of the list items.

Copy and paste the following code just beneath `//Code Block 4`:

```JavaScript

li.addEventListener('click', function(){
    loading.dataset.active = "true";
    const elevatorId = this.dataset.id;
    console.log(elevatorId);
    
    document.querySelector('#title').textContent = elevatorId;
    elevatorHolder.dataset.active = "false";
    
    fetch(`${APIRoot}/events/device/${elevatorId}`)
        .then(res => {
            if(res.ok){
                return res.json();
            } else {
                throw res;
            }
        })
        .then(response => {
            
            const data = response.data;
            currentData = data;
            console.log(data);
            
            // Code Block 5

            back.dataset.active = "true";
            loading.dataset.active = "false";
            
        })
        .catch(err => {
            console.log('err:', err);
        })
    
}, false);
```

Deploy your changes, and reload your web page. If you've closed them, reopen your developer tools by right-clicking on the page and clicking `inpsect` then select the 'Console' tab. Now, when you click on an elevator in the list, the list will be replaced by a loading wheel and our code will go and get all of the events for the elevator that have ever occured. In our console window something like the following should be returned after a few seconds:
```
(6408) [{…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, …]
```

That's all of the data for this elevator 🎉

9. So let's do something with it! Before we go any further, we need to create a few helper functions to help us navigate around the app - namely, a back button. Right back at the start of our JavaScript, there's a variable `currentDataOffset`, copy and paste the following code, just after that line

```JavaScript
back.addEventListener('click', function(){
    this.dataset.active = "false";
    infoHolder.dataset.active = "false";
    elevatorHolder.dataset.active = "true";
}, false);

// Code Block 6
```

Deploy those changes. Now when you click on an elevator in the list, we'll still get the data for that elevator, but you're also able to browse back to the list of elevators to see info for a different device.

10. Now it's time to display the data for our elevators. We don't want to write and rewrite code that does the same (or similar) things as other bits of code, so we're going to create two helper functions that we'll use to construct the HTML:

    - `displayEventData`
        - This function we will call to add data to our page and bind events for interactivity
    - `constructHTMLForEventData`
        - This function will parse an individual events data and create and populate the required HTML to display information.

11. First, we'll create the `constructHTMLForEventData` function. Head back to your JavaScript template node and copy and paste the following code on the line after `// Code Block 6`:

```JavaScript
function constructHTMLForEventData(eventData){
                            
    const desirableProperties = {
        "TS_TIMESTAMP": "Datetime",
        "motorTemp": "Motor Temperature",
        "currentFloor": "Current Floor",
        "doorOpen": "Door Open?",
        "state": "State",
        "cabinTemp": "Cabin Temp. (F)",
        "cabinSpeed": "Speed",
        "direction": "Direction",
        "load": "Load (kg)"
    };
    
    const item = eventData;
    const frag = document.createDocumentFragment();

    const title = document.createElement('h2');
    title.textContent = 'Data'

    const range = document.createElement('input');
    range.setAttribute('type', 'range');

    frag.appendChild(title);
    frag.appendChild(range);
    
    Object.keys(desirableProperties).forEach(key => {

        const value = item[key];
        
        if(value){
            
            const li = document.createElement('li');
            const strong = document.createElement('strong');
            const p = document.createElement('p');
            
            strong.textContent = desirableProperties[key] + ":";
            p.textContent = item[key];
            
            li.appendChild(strong);
            li.appendChild(p);
            
            frag.appendChild(li);
            
        }
        
    })
    
    return frag;
}

// Code Block 7

```

This function takes the data for an individual event, filters the information for the properties that we want to display, and then creates a document fragment (basically a mini DOM) with DOM nodes that contains each bit of useful information.

12. The `constructHTMLForEventData` function isn't called directly by any piece of code that we write, it's used by the `displayEventData` function that we'll trigger in the next few steps. Copy and paste the following code just after `// Code Block 7`:

```JavaScript

function displayEventData(event){

    infoHolder.dataset.active = "true";
    info.querySelector('#data').innerHTML = "";                    

    const frag = constructHTMLForEventData(event);
                        
    const range = frag.querySelector('input[type="range"]');
    range.value = currentRangeOffset;
    
    range.addEventListener('change', function(e){
        console.log(this.value);
        
        currentRangeOffset = this.value;

        const arrayOffset = this.value / 100 * currentData.length | 0
        console.log( arrayOffset );

        const newData = currentData[ arrayOffset ];

        if(newData){
            displayEventData(newData);
        }

    }, false);
    
    info.querySelector('#data').appendChild(frag);

}
```
This function, though shorter than the `constructHTMLForEventData`, does a little bit more. Having called the `constructHTMLForEventData` function, `displayEventData` then uses a querySelector to retrieve the range input which we'll use to scrub through the event data in our UI. Once it has a reference to that, it binds an event listener to the input that will trigger whenever the user drags it to select a new point in time.

13. We're almost very ready to to view our data! Just one more line of code to copy and paste and then we're good to go! Grab the following line and paste it on the line just after `// Code Block 5`

```JavaScript
    displayEventData(data[0]);
```

Deploy your app, reload the page, select an elevator and see the individual event data for this elevator!

## That's all folks!

We've built a web app the view historical IoT data. Of course, the application can take any form - it could be a mobile phone or desktop app, we've built a web becuase it's a great, accessible technology.

## Addendum
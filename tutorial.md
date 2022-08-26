# API tutorial
This tutorial shows you how to access a simple API with the JavaScript `fetch` command.
- Set up your basic page without any actions. Notice that we have put identifiers for each element.
```html
<html>
    <head>
        <title>City Finder</title>
    </head>
    <body>
    <form>
        Enter A Utah City: <input type="text" id="cityField" value=""><br>
        Suggestion: <span id="txtHint">Empty</span>
        <input id="weatherButton" type="submit" value="Submit">
    </form>
    <p>City</p>
    <textarea id="displayCity">No City</textarea>
    <p>Current Weather</p>
    <div id="weather">No weather</div>

    </body>
</html>
```

- Then make sure you can catch the <a href="http://api.jquery.com/keyup/">keyup event</a> and open an alert box. Put this code at the bottom of your page so that the buttons will be in the document before you try to catch the event.

```html
<script>
document.getElementById("cityField").addEventListener("keyup", function(event) {
  event.preventDefault();
  alert( "Handler for .keyup() called." );
});
</script>
```

- Now prove to yourself that you can modify the txtHint span
```js
document.getElementById("cityField").addEventListener("keyup", function(event) {
  event.preventDefault();
  const value = "Keyup";
  const txtHint = document.getElementById("txtHint");
  txtHint.textContent = "";
  txtHint.appendChild(document.createTextNode(value));
});
```
- Now show that you can get the value from the cityField form.
```js
document.getElementById("cityField").addEventListener("keyup", function(event) {
  event.preventDefault();
  const value = document.getElementById("cityField").value;
  const txtHint = document.getElementById("txtHint");
  txtHint.textContent = "";
  txtHint.appendChild(document.createTextNode(value));
});
```
- At this point you have a dynamic page, but you would like to get some real data to put into the Suggestion span.  Lets start with a baby step and create some fake <a href="http://www.json.org/">JSON</a> data that we can pretend came from a <a href="https://github.com/tfredrich/RestApiTutorial.com/raw/master/media/RESTful%20Best%20Practices-v1_2.pdf">REST</a>ful service.
<p>
Create a file <a href="http://students.cs.byu.edu/~clement/CS360/jquery/staticCity.txt">staticCity.txt</a> on your web server with the following content:

```
[
  {"city":"Provo"},
  {"city":"Lehi"}
]
```

You will want to make sure you can read this  array of two city entries before you talk to a live REST service.
```js
const url = "staticCity.txt";
fetch(url)
  .then(function(response) {
    return response.json();
  }).then(function(json) {	
    console.log(json);
    console.log(json[0]);
    console.log("Got " + json[0].city);
  });
```

You ought to be familiar with using console.log in conjunction with the <a href="https://developer.chrome.com/devtools">javascript console</a> in your browser to debug your code.

Open the console in your chrome debugger to see the data that is returned from the `fetch` call.

- Now let's write the response as an unordered list into the Suggestion span with id #txtHint.

```js
const everything = document.createElement("ul");
for (let i = 0; i < json.length; i++) {
  const value = json[i].city;
  const item = document.createElement("li");
  item.appendChild(document.createTextNode(value));
  everything.appendChild(item);
};

const txtHint = document.getElementById("txtHint");
txtHint.textContent = "";
txtHint.appendChild(everything);
```
- Now it is time to call a real RESTful service. Found here:
                                
```
https://csonline.byu.edu/city
```
                                
There are a lot of things that could go wrong, so it is a good idea to take baby steps. This service takes a query parameter following ? in the URL.  So lets start by passing it a "P" to get all cities that start with a P.

```html
<script>
document.getElementById("cityField").addEventListener("keyup", function(event) {
    event.preventDefault();
    const url = "https://csonline.byu.edu/city?q=P";
    fetch(url)
        .then(function(response) {
            return response.json();
        }).then(function(json) {
            console.log(json);
            console.log(json[0]);
            console.log("Got " + json[0].city);
            const everything = document.createElement("ul");
            for (let i = 0; i < json.length; i++) {
                const value = json[i].city;
                const listItem = document.createElement("li");
                listItem.appendChild(document.createTextNode(value));
                everything.appendChild(listItem);
            };
            const txtHint = document.getElementById("txtHint");
            txtHint.textContent = "";
            txtHint.appendChild(everything);
        });
});
</script>
```
- Now we want to pass it the real characters from the form to the REST service.  We will append the characters the user has typed to the end of the URL.

```js
const url = "https://csonline.byu.edu/city?q=" +
      document.getElementById("cityField").value;
```

- Now you should have a fully working Hint system.  Congratulations!
</ol>

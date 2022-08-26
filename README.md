# Using APIs
For this assignment, you will find a REST service and create an application that uses it.

You can find a large number of REST services 
[here](https://github.com/toddmotto/public-apis)

You will want to use the ones that do not have [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) issues.  Avoid these and make sure you can access the REST service from your javascript code before you put a lot of time into developing the rest of your application.  Some of these REST services could also be investigated.

* [Google APIs](https://code.google.com/apis/)
* [rapidapi](https://rapidapi.com/)
* [any-api](https://any-api.com/)
* [apilist](https://apilist.fun/)

Some of these services will not work or will require money, so you will have to explore a little.

Once you have found a service you want to use, you need to figure out how to make the ajax call.

For this example, let's assume you want to display the xkcd comics. The following steps might be useful.

First find the interface. I googled for "rest api xkcd" and found this documentation
Then I created a small chunk of html  to make sure I understood the interface.
But it doesn't work. I get the error:
XMLHttpRequest cannot load http://xkcd.com/info.0.json. No 'Access-Control-Allow-Origin' header is present on the requested resource. 
This occurs because the xkcd.com/info.0.json interface is not setting the "Access-Control-Allow-Origin:" field to allow web pages from other sites to access the REST interface (see the wikipedia article)
One way of getting around this is to use a proxy server like cors-anywhere (Links to an external site.) that will insert the CORS headers. (NOTE: You may need to enable cors-anywhere for your browser. You can do that by going to https://cors-anywhere.herokuapp.com/corsdemo .) (Links to an external site.)
So I change my code to make a request through the proxy in this chunk of html and it works.  Notice that I used the proxy, and I inserted the "{mode: 'cors'}" parameter into the fetch.
I always bring up the chrome developer tools to inspect the object that is returned by a service so I will know what fields I can access.
I could also google for "xkcd CORS API" and found this URL (Links to an external site.) that also provides the CORS header.

https://xkcd.now.sh/?comic=latest
Here is a working repo (Links to an external site.) with this version of the code.

You could also get the image by creating an image tag with something like this:

let everything = "<p>";
everything += json["alt"];
everything += "</p>" + '<img src="'+json["img"]+'"></img>"';
document.getElementById("comic").innerHTML = everything;
Hopefully, this gives you an insight into what to do to connect to a REST service.

Create a web application that accesses data from xkcd or one of these other web services

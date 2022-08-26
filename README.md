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

1. First find the interface. I googled for "rest api xkcd" and found this [documentation](https://xkcd.com/json.html).
2. Then I created a small [chunk of html](https://github.com/BYU-CS-260/APItutorial/blob/main/xkcdCORS.html) (try it out on your own server) to make sure I understood the interface.
But it doesn't work. I get the error:
```
XMLHttpRequest cannot load http://xkcd.com/info.0.json. 
No 'Access-Control-Allow-Origin' header is present on the requested resource.
```
This occurs because the xkcd.com/info.0.json interface is not setting the "Access-Control-Allow-Origin:" field to allow web pages from other sites to access the REST interface (see the [wikipedia article](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing))

3. One way of getting around this is to use a proxy server like [cors-anywhere](https://cors-anywhere.herokuapp.com/) that will insert the CORS headers. 
(NOTE: You may need to enable cors-anywhere for your browser. You can do that by going to [https://cors-anywhere.herokuapp.com/corsdemo](https://cors-anywhere.herokuapp.com/corsdemo))

So I change my code to make a request through the proxy in this [chunk of html](https://github.com/BYU-CS-260/APItutorial/blob/main/xkcdWORKS.html) and it works.  Notice that I used the proxy, and I inserted the "{mode: 'cors'}" parameter into the fetch.

I always bring up the chrome developer tools to inspect the object that is returned by a service so I will know what fields I can access.

4. I also googled for "xkcd CORS API" and found this [URL](https://xkcd.vercel.app/?comic=latest) that also provides the CORS header.
```
https://xkcd.vercel.app/?comic=latest
```
Here is a working [repo](https://github.com/BYU-CS-260/APItutorial/blob/main/xkcdvercel.html) with this version of the code.

You could also get the image by creating an image tag with something like this:
```
let everything = "<p>";
everything += json["alt"];
everything += "</p>" + '<img src="'+json["img"]+'"></img>"';
document.getElementById("comic").innerHTML = everything;
```
Try this out to create a xkcd viewer page on your server.

Hopefully, this gives you an insight into what to do to connect to a REST service.

Create a web application that accesses data from xkcd or one of these other web services

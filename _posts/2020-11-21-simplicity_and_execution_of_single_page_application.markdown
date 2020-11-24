---
layout: post
title:      "SPA,Fetch,Promise!"
date:       2020-11-21 18:51:15 -0500
permalink:  simplicity_and_execution_of_single_page_application
---

```SPA ->``` 
Creating a frontend js application is differant than any other projects. The main differance is number of html view files we use in a single page application known as 'spa'. In sinatra or rails application we had to make a html file for every http request we made in our app to show the view on the screen. Js has given us the opportunity to use only one html file to render everything on the DOM. In Js application, when we click on something to view, the previous view on the DOM gets cleared and the DOM gets loaded with new contents. In js application, usually there are two parts connected with each other. One is frontend and the other one is backend. For the frontend we use html and js. On the other hand, we use rails api as a backend. 

```Fetch ->```
Whenever we need something from the backend, we make a AJAX call using fetch. Inside the fetch request, we provide the url where we want to make a fetch request. fetch() uses an HTTP GET to retrieve the content specified by a URL and HTTP POST to send content gathered in a JavaScript Object. A basic fetch request looks like this -> 
```fetch(http://localhost:3000/kingdoms)```. Over here, we are making a fetch request to the backend to get all the kingdoms. Since we haven't specified any method type, the default request type is get method. When the program executes this line, the program tries to match the routes starts with a get request and a url like '/kingdoms'. The route looks like this-```get '/kingdoms'```. After finding this route, the associated method with this route in the controller is being called. To get all the items from the backend usually index method is called. This method requests to the backend to fetch all items and store it in a local variable. To store all kingdoms in a varaible called kingdoms we have to do ```  kingdoms = Kingdom.all ```. Later that variable is rendered as a json object on the screen which is executed by ```render json: kingdoms ```. The serializer file specifies the attributes that can be viewed on the screen.

Usually for a get request, fetch takes only one argument as a string. However, fetch() can also take a JavaScript Object ({}) as the second argument. This Object can be given certain properties with certain values in order to change fetch()'s default behavior. Then the fetch request becomes ```fetch(destinationURL, configurationObject)```. The configurationObject contains three core components that are needed for standard POST requests. To make a fetch request with a post/update/delete method we add a method key to our configurationObject. For a post request, 

```configurationObject = {
           method: "POST",
	         body: JSON.stringify(),
	         headers: {
                'Content-Type': 'application/json',
                'Accept': 'application/json'
           }
     };``` 

Headers are sent just ahead of the actual data payload of our POST request. They contain information about the data being sent. Each individual header is stored as a key/value pair inside an object. Just like "Content-Type" tells the destination server what type of data we're sending, it is also good practice to tell the server what data format we accept in return. Also, Data being sent in fetch() must be stored in the body of the configurationObject which is the last component. By passing an object in, JSON.stringify() will return a string, formatted and ready to send in our request. 

```Resolve as response ->```
After the fetch request, servers will send a [Response]. To access the information from the response that we get back, we use a series of calls to then() which are known as function callbacks. The returned response is converted into json then. ```.then(response => response.json())```. The method json() converts the body of the response from JSON to a plain old JavaScript object. The result of json() is returned and made available in the second then(). 

```.then(function(object) { 
          console.log(object);
      }) ```
			
``` Reject as catch() ->```
When something goes wrong in a fetch() request, JavaScript will look down the chain of .then() calls for a catch. When something goes wrong in a fetch(), the next catch() is called so that error work can be performed. If we forgot to add the HTTP verb to our POST request, and the fetch() defaults to GET. By including a catch() statement, JavaScript doesn't fail silently. 

```.catch(function(error) {
    alert("Bad things! Ragnarők!");
    console.log(error.message);
  }); ```
	
	```Promise ->```
	
Whenever we make any kind of request to the backend, we get a promise back. At that stage, the promise is in a pending state. If the promise is resolved then() callback function is executed and we get a response back. Otherwise, the promise is rejected and program of execution goes to the catch block to display the error by skipping  those lines inside then function. If everything works fine, we get the result as a json object and render it on the DOM. 

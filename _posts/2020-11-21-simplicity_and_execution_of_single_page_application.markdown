---
layout: post
title:      "Simplicity & Execution of Single Page Application!"
date:       2020-11-21 23:51:14 +0000
permalink:  simplicity_and_execution_of_single_page_application
---


Creating a frontend js application is differant than any other projects. The main differance is number of html view files we use in a single page application. In sinatra or rails application we had to make a html file for every link we had in our app to show the view on the screen. Js has given us the opportunity to use only one view file to render everything on the DOM. In Js application, when we click on something to view, the previous on the DOM gets cleared and the DOM gets loaded with new contents. In a js application, usually there are two parts connected with each other. One is frontend and the other one is backend. For the frontend we use html and js. On the other hand, we use rails api as a backend. Whenever we need something from the backend, we make a AJAX call using fetch. Inside the fetch request, we provide the url where we want to make a fetch request. 

A basic fetch request looks like this -> 
```fetch(http://localhost:3000/kingdoms)```. Over here, we are making a fetch request to the backend to get all the kingdoms. Since we haven't specified any method type, the default request type is get method. When the program executes this line, the program tries to match the routes with a get request method with a url like '/kingdoms'. ```get '/kingdoms'```. After finding this route, the associated method with this route in the controller is being called. To get all the items from the backend usually index method is called. This method requests to the backend to fetch all items and store it in a local variable. ```  animals = Animal.all ```. Later the instances in that variable is rendered as a json object on the screen.```render json: animals ```. The serializer file specifies the attributes that can be viewed on the screen.

After the fetch request, we get a response back from the controller. ```.then(response => response.json())```. Then the response is converted into json. At the end, we get the result as a json object and render it on the DOM. 

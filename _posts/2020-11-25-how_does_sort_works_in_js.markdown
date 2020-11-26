---
layout: post
title:      "How does sort works in JS"
date:       2020-11-25 20:17:24 -0500
permalink:  how_does_sort_works_in_js
---


In a js application we might be in a situation where we need to sort out something that is already renderred on the page. They can be any elements like names or anchor tags. Suppose, whenever on the DOM when we see a list of items fetched from the backend, we want to sort the items after clicking on a button called 'sort'. To make things work, we need 4 functions basically. one function to fetch all the elements from the backend and to add a button called 'sort', one to render the items on the DOM, one to handle the 'submit' button event and the last to compare two elements based on the sorting condition.

```Fetch everything from backend and add sort button ->``` When we make a request to the backend to fetch something we use fetch. Inside the fetch call, we provide the basic url + the route. So, if we want to fetch all the animals from the backend, the fetch will be ```fetch('http://localhost:3000/animals')```. 

```document.getElementById("animals").addEventListener('click', fetchAllAnimal)

function fetchAllAnimal(){
 fetch(BASE_URLS + '/animals')
    .then(response => response.json())
    .then(animals => {
		     animals.forEach(ani => { 
            const an =  new An(ani)
            main_animal.querySelector("ul").innerHTML += an.renderAnimalName()
         })```
				 
We have fetched all the animals from the backend and created an instance of the animal class for each animal. We have stored all the inastances of animal class inside a static array called collection. 

```class An {
    static collection = []
    constructor(animal){
		 An.collection.push(this)
    }``` 
		
```Render items on the DOM ->``` After creating the inastance, we are renderring the items on the DOM by calling the function 'renderAnimalName()'. 

```renderAnimalName(){
        return (`<li id="animal-${this.id}">
                    <a href="" data-id="${this.id}">${this.name}</a>  
										</li>`
                )
    }``` 
		
``` Adding sort button ->``` 

``` const html = `<button id="sort">sort</button>`
        createAnimalFormDiv.innerHTML = html ```
				
```Event Handler for sort button ->``` When we click on a sort button after viewing all the items on the DOM, the event handler for that sort button is handled by a function called myFunc. Then we call the static array from the animal class to get all the instances of the animals and call the sorting called 'compare'. After sorting, we have to re render.

```function myFunc(){
    An.collection.sort(compare) 
		An.collection.forEach(a => {
        main_animal.querySelector("ul").innerHTML += a.renderAnimalName()
 })
}``` 

```Sort Function Comapre ->``` The sort function two items as an argument and sorts it based on the given condition. If we want to sort the animals by name the scenario look like 

```function compare(a,b){
    const nameA = a.name.toUpperCase();
    const nameB = b.name.toUpperCase();

  let comparison = 0;
  if (nameA > nameB) {
    comparison = 1;
  } else if (nameA < nameB) {
    comparison = -1;
  }
  return comparison *;
}``` 

If we want to sort the elements by desc order, we have to multiply comparison with -1in return statement. After sorting, the function returns its result to the static array. Finally, We render the sorted items on the DOM.
		

				 
			
			
			
			
				 


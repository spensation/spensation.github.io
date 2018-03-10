---
layout: post
title:      "I'll make dinner, I promise"
date:       2018-03-10 20:07:04 +0000
permalink:  ill_make_dinner_i_promise
---

## How async JS works like an efficient chef

If you have ever made a nice dinner for your friends or family, you can understand how asyncronous JavaScript works.

First of all, we need to establish that JS is a synchronous programming language, meaning it runs one line of code at a time. It cannot move on to the next line of code until the current line has finished executing.  for example:

```
function syncMakeDinner() {
  console.log('Roast a chicken');
  console.log('make a side dish');
  console.log('make a salad');
}
syncMakeDinner();
/* Output appears in the same order it was written:
   Roast a chicken
   make a side dish
   make a salad
	 serve wine
*/

```

Now imagine if I made dinner in this syncronous fashion.  I would start with roasting the chicken.  First, I have to pre-heat the oven to 425 deg.  Only then can I really start cooking the chicken.  I would also want to baste the chicken in its juices at some point during the cooking process.  Finally I have to remember to take the chicken out when it is ready.  All that could take up to 90 minutes.  So for an hour and a half, I'm sitting in the kitchen tending to the bird before I can move on to the next task.  

here's how that might look in code:


```
function roastChicken() {
   preheatOven();
	 startCookingChicken();
	 basteChicken();
	 removeChicken();
}

function makeSideDish() {
   // imagine more code here
}

function makeSalad(); {
   // imageine even more code
}

roastChicken();
//  Now wait up to 90 minutes before completing other functions

makeSideDish();
makeSalad();
serveWine();
	 

```

That's not gonna get my family fed any time soon!  Any home chef worth their salt would tackle these tasks asynchronously.  

This action of going back to the chicken when it’s ready is analogous to the JavaScript callback function, and our oven can quite literally call us back with a timer! This allows us to go on and do other tasks and then come back to the chicken when it is ready for us.  

### Asyncronous Make Dinner

Lets start over and this time we will work asynchronously.

`function roastChicken(callback) {
  // imagine initial code that pre-heats the oven
  // this could take 10 minutes
  callback(err, preheatOven);
}
roastChicken(function(err, preheatOven) {
  // sometimes our oven goes haywire 
  // better handle that possible error
  if (err) throw err;
  // if no errors, put chicken in the oven after it reaches temperature
  // Tada! Our call back alerting us that the oven is hot!
  startCookingChicken(preheatOven);
});
// as we wait, JavaScript will run this stuff now!
makeSideDish();
// I'm already way ahead of schedule!
makeSalad();
// Now the chicken needs to be basted! 
// Let's go to the oven and take care of that
// The chicken is bake in the oven. Let's start clening up some of the mess we have made.
// My family will be impressed with my productivity!
serveWine();``

```


### Move over Callback, here comes the Promise

Now that we have a general idea of how async works with callbacks, lets talk about the Promise object.   Callback functions have a tendency to get complicated.  Imagine  a longer list of tasks that all rely on each other's status to get completed. Now imagine that all those tasks have callback functions.   That is known as Callback Hell.  

A promise is an object that will return a value in future. Because of this “in future” thing, Promises are well suited for asynchronous JavaScript operations.

It might look like this:

```
getSomethingWithPromise()
  .then(data => {/* do something with data */})
  .catch(err => {/* handle the error */})
  
```

One of the most common places where JS gets asyncronous is when we make a call to an API.  Lets say we use Fetch to grab some JSON from an API.  While the fetch request is being handled, JS has moved on to the next executable line of code.  Continuing with the making dinner theme, let's say we need to find a recipe.  We make a call to an API.    For the purpose of this example. lets just say we are going to console.log the response:

```

callApi() {
		fetch('/api/v1/recipes')
    		.then(response => {return response.json()})
    		.then(recipes => console.log(recipes))
    		.catch(err => console.log(err))
				
		// some other executable line of code
	}	

```

The Promise is comprised of handlers, like then() and catch(),  that chain together to process the request.
In our recipe example above, the fetch request accesses the api, requsts the response in JSON, then takes  that response and logs it to the console.  In the event of an error, there is a catch handler that will log an error message.

We can add some more console.logs in order to clarify the flow:

```
callApi() {
		console.log('a')
		fetch('/api/v1/recipes')
    		.then(response => {
    			
				console.log('b')
    			return response.json()
    		})
    		.then(recipes => console.log('c', recipes))
    		.catch(err => console.log('d', err))
			console.log('e')
	}
```
Assuming there are no errors, the result callAPI(); would be: "a, e, b, c, recipes"



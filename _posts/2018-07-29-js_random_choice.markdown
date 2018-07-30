---
layout: post
title:      "JS Random Choice"
date:       2018-07-29 23:49:45 -0400
permalink:  js_random_choice
---


I was recently working on a gamelike project just for fun.  It was an attempt to understand flexbox, which is a whole other story.  Anyway, I wound up building a little game that has three balls bouncing around and when you mouseover the ball, the css layout changes.  I wanted to add an element of color changing to it and I leaned on some javascript to make it happen.

## The Structure:

The html elements that I am working with are really basic.  There are mouseover events on three functions that are defined in the script tag; fun(), andFun(), and moreFun().  They are all bound to this, or the mouseover event.

*index.html*
```
<div id="box-container">
   		<div id="box-1" class="blue">
   		   <div class="balls" id="left" onmouseover="fun(this)"></div>
			   <div class="balls" id="center" onmouseover="andFun(this)"></div>
			   <div class="balls" id="right" onmouseover="moreFun(this)"></div>
   		</div>
			
  		<div id="box-2" class="orange"></div>
</div>

 <div id="box-container-2">
   		<div id="box-3" class="blue"></div>
  		<div id="box-4" class="orange"></div>
	</div>
```


##  The Style:

In the stylesheet, I have a few properties set to each container and box item, respectively.:

*style.css*
```
 #box-container {
	    display: flex;
	    height: 12vh;
	    justify-content: center;
	  }
	  #box-container-2 {
	    display: flex;
	    height: 12vh;
	    justify-content: center;
	  }
		
		 #box-1 {
	    background-color: dodgerblue;
	    flex: 1.5 1.5 auto;
	    height: 12vh;
	    
	  }

	  #box-2 {
	    background-color: orangered;
	    flex: 1 1 auto;
	    height: 12vh;
	    
	  
	  }
	  #box-3 {
	    background-color: dodgerblue;
	    flex: 1.75 1.75 auto;
	    height: 12vh;
	    		   
	  }

	  #box-4 {
	    background-color: orangered;
	    flex: 1 1 auto;
	    height: 12vh;
	    	    
	  }
```

Essentially, I have four flexboxes that are of cetain dimensions and are either 'dodgerblue' or 'orangered'.  I am also setting the height constraints with  I will go over the theory behind the flexbox in another blog post but for now lets focus on the javascript.


## The Javascript:

Unlike in Python, I can't just import random and call random on an array.  So, what do we do?

We will focus on the function that controls the color changing in this example, moreFun().  

Inside the function, we start by defining an array of colors:

```

	function andFun(e) {
		const colors = [
			"black", 
			"red", 
			"aquamarine", 
			"green",
			"lime",
			"gray",
			"darkmagenta",
			"firebrick",
			"seagreen",
			"skyblue"
		];

```

Next we can do the math!  We will define a variable of the random choice that will select one of the colors in the array by its index.  To generate a random index number we call Math.random, which return a number between 0 and 1, and multiply that the length of the array.  We also have to use Math.round to round the result to the nearest integer.  Put it all together and it looks like this:

```
const randomChoice = colors1[Math.floor(colors.length * Math.random())];
```


Now we are ready to assign one of these random colors to a set of my box elements. In order to do this, we will create a variable and set it to a query selector 


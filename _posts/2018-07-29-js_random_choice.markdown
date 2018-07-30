---
layout: post
title:      "JS Random Choice"
date:       2018-07-30 03:49:44 +0000
permalink:  js_random_choice
---


I was recently working on a gamelike project just for fun.  It was an attempt to understand flexbox, which is a whole other story.  Anyway, I wound up building a little game that has three balls bouncing around and when you mouseover the ball, the css layout changes.  I wanted to add an element of color changing to it and I leaned on some javascript to make it happen.

## The Structure:

The html elements that I am working with are really basic. 

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

In the stylesheet, I have a few properties set to each contaier and box item, respectively.:

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




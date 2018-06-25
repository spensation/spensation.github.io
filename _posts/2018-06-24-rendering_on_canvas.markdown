---
layout: post
title:      "Rendering on Canvas"
date:       2018-06-24 23:46:40 -0400
permalink:  rendering_on_canvas
---


One of the things that I have been working on lately is rendering shapes on a canvas HTML element and then animating them with JavaScript.  Ultimately, I want to be able to draw fractals, but let's take it one step at a time.

## Setting up a canvas

The canvas tag, like any HTML element that you want to display on your page, must be placed in the body of your index.html file.  

*from index.html*
```

<!DOCTYPE html>
<html>
<head>
	<title>Render Elements!</title>
	<link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
	<h1>Render Elements!</h1>
	<canvas></canvas

</body>
</html>
```

Now, what are we going to put in our canvas?

We start by setting some basic attributes, height and width.  

*from index.html*
```
...
<body>
	<h1>Render Elements!</h1>
	<canvas height="300" width="300"></canvas

</body>
...
```
Now we have a canvas that is 300 px square.  We still can't see it on the page, but it's there.

If we add a little styling it will all become clear.  I'm writing this css to a style.css file that is linked to my index.html file.  (see snippet above)


*from style.css*
```
body {
	text-align: center;
	background-color: white;
}

canvas {
	margin: auto;
	background-color: black;
}

```

## Adding Elements

Now I have a black canvas sitting centered against a white page.
In order to render to the canvas, we will need to use JavaScript.

For simplicity's sake, let just add a script tag to our index.html file.  If this was any more complicated than getting a shape to the page, I would create a seperate file called render.js, or something like that, and create another relative link in the head of the html file (like we did for the style.css file)


*from index.html*
```
<!DOCTYPE html>
<html>
<head>
	<title>Render Elements!</title>
	<link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
	<h1>Render Elements!</h1>
	<canvas height="300" width="300"></canvas>

  <script>
	  // js magic goes here
	
  </script>
</body>
</html>
```

Since we are now in JS land, what's the first thing we need to do?   Declare a variable!  

*from index.html*
```
...
  <script>
	 let canvas = document.querySelector("Canvas")
	 let ctx = canvas.getContext("2d");
	
  </script>

```

To be more specific, we declare "canvas" to be a DOM element by using document.querySelector() and passing in "Canvas".  Now we can call getContext() on canvas and pass in "2d".  There are other options for 3D and bitmap rendering but for our puposes, 2d is what we want to pass into this method.

Now we are ready to add whatever elements we want.  Rectangles, arcs. circles, lines and text are all possible.  For a simple example, let's render a red square.

*from index.html*
```
  <script>
	 let canvas = document.querySelector("Canvas")
	 let context = canvas.getContext("2d");
	 
	 context.fillStyle = "red";
   context.fillRect(10, 10, 100, 50);
	
  </script>
```

We first call fillStyle on context and set it equal to red.  Pretty straightforward.  Next. we call fillRect (to give us a rectagular element) and give it some pearmeters.
The first and second parameters are for the left and top margin, respectively.  This element's upper left corner will be 10 pixels from the left and 10 pixels from the top of the canvas.  The third parameter defines the width and the fourth the height.  So we will have a red rectangle 100 pixels wide and 50 pixels high.


This is just the beginning of being able to manipulate custom shapes on your webpages or apps.  





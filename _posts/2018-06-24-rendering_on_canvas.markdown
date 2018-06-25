---
layout: post
title:      "Rendering on Canvas"
date:       2018-06-25 03:46:39 +0000
permalink:  rendering_on_canvas
---


One of the things that I have been working on lately is rendering shapes on a canvas HTML element and then animating them with JavaScript.  Ultimately, I want to be able to draw fractals, but let's take it one step at a time.

## Setting up a canvas

The canvas tag, like any HTML element that you want to display on your page, must be placed in the body of your index.html file.  

```
<!DOCTYPE html>
<html>
<head>
	<title>My Game</title>
	<link rel="stylesheet" type="text/css" href="style.css">
</head>
<body onload="startGame()">
	<h1>Render Elements!</h1>
	<canvas></canvas

</body>
</html>
```
Upon 






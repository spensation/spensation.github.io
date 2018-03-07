---
layout: post
title:      "HTML Basics"
date:       2018-03-07 18:24:04 +0000
permalink:  html_basics
---


Let's start at the beginning.  Setting up an html web site is pretty straight-forward.  Start by opening a new file in your favorite editor, like Atom or Sublime text.  

Then create a new file called index.html.  This will be your main page.  

The first thing to do is write the DOCTYPE tag at the top of the page.  

``
<!DOCTYPE html>`
<html lang="en">

```

This tells the browser that this file is is html file and that it is in the english language.  If I wanted to publish a page in French, par example, I would change the 'en' to an 'fr'.  Then I could work in Paris!

The first thing to know about html tags is that most of them need to be 'opened' and 'closed'.  What does that even mean?  Well, let's look at an <h1> tag.  This is the headline tag and will default to the largest text available on your page.  (As opposed to h2, h3, h4 , h5, and h6 tags which get smaller and smaller).  Let's use an <h1> tag to shout Hello, World!.

```
<h1>Hello, World!</h1>
```

Above you wil see that the two tags are slightly different.  <h1> opens the Headline, and </h1> closes it.  The content is located between the two tags.  




HEAD AND BODY:


There are two very important sections of your new website, the Head and the Body and they couldn't be more different.  

The head section goes at the top of the page and it contains all the meta-data and dependencies that your site needs to operate and be searchable on the web.  It's content is not seen by the user.

```
<head>
		<meta charset="utf-8">
		<title>My Awesome Website!</title>
		<link rel="stylesheet" type="text/css" href="style.css">
	</head>
```

This example has the the meta character set tag set to 'utf-8', the standard setting.  It also has a title, 'My Awesome Website', and it has a link to a stylesheet.  Don't worry too much about that right now, CSS and how to create a link to an 'external style sheet' will be addressed in an upcoming post.  


The Body is the section that the user can see and interact with.  It contains all the text, images and links.  All that content goes inside the body tag.

```
<body>
    <h1>Welcome to my awesome Website!</h1>
</body>
```

Now the possibilities are limitless.  Most sites have a navbar, so you can navigate to other pages of the site.

That could look a little something like this:

 ```
<div class="navbar">
			<a href="index.html">Home</a>
			<a href="http://myblog.com/">Blog</a>
			<a href="projects.html">Projects</a>
			<a href="resume.html">Résumé</a>
		</div>
		
```

Let's break down this navbar.

The 'div' tag creates a physical space for this content to hold on the page.  Pretty much every unique element of your page will be inside a div tag.

This particular div tag has been assigned a class of 'navbar'. With this attribute defined, I can access this div in my CSS file and make it look awesome.  This will become very important in the near future!

The next new thing we see is the 'a', or anchor tag.  This operates as a link.  It has an attribute called href, or hypertext reference.  Set the href equal to the destination of the link.  In this example, the "Home" link takes you to the index.html file that we are building right now.  There are also links to a Projects page and a Resume page that are 'relative' paths within this file structure.  The link to the Blog is an 'external link that goes to a page on the internet, hence the "http://.." address.  

The next step might be to create another page of your website, like the resume page.  Call it 'resume.html.  Remember to start with the <DOCTYPE> tag.  Also just copy and paste your <head> section over to the new page.  Your going to need that.  Open a body tag and put some new content in there...

```
<body>
    <h2>My Resume...>/h2>
</body>
```

If everything is working correctly, you should be able to navigate from your index page to your resume page and see some content.  Awesome!  You're on your way to building a website!  


This is just a brief summary of the very beginning of HTML coding.  There is so much more to learn!











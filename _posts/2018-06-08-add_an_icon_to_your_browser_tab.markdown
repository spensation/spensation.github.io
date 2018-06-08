---
layout: post
title:      "Add an icon to your browser tab"
date:       2018-06-08 17:08:38 +0000
permalink:  add_an_icon_to_your_browser_tab
---


In an effort to make my portfolio site look pro, I had to figure out how to get in icon into the browser tab.  It turns out that it is pretty simple.  First, you need an icon.  I looked at logomakr.com's library of icons but I didn't find anything that really spoke to me so I decided to make my own.  I'm a mac user, so I started in pages.  I designed a logo then exported it as a pdf to preview, where I edited out to background with the magic wand tool.  I was then able to change the format to png and export it to the images file for my project.

Now that the image was in my project folder, I was ready to code!

Inside the <head> tag I created a new <link> tag to hold the icon.  The relationship (rel) is set to "icon", because that's what I am linking to my index.html page.  Next, we set the type to "image/png".  Finally, I target the href, or the hyper link reference.   Because my index.html file sits in a seperate folder than my images folder, I had to set the href equal to a relative path.  

index.html:

```
<head>
      <link rel="icon" type="image/png" href="./images/your_logo.png">
</head>
```

And that's it!  Don't forget to copy your new link tag into any other pages in your site!

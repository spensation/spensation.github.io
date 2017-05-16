---
layout: post
title:  Ruby scraping CLI app assignment.  
date:   2017-05-16 16:52:09 +0000
---


This CLI assignment has been quite a challenge.  Let me start by saying that I have a newborn son.  He is six weeks old now and my wife and I are just starting to get our feet under us.  Suffice it to say that my studies have suffered.  The crux of the matter is that writing code, or learning how to write code, requires deep focus and attention.  This is not possible with a newborn in the house.  Anyway, we are back on the right track now.  

Now that I have my first rubygem under my belt, I feel pretty proud, in a very humble way.  It took a few tries to get right, I am not ashamed to admit.  I wanted to build a gem that showed the dance performance listings in the Bay Area. (Before I decided to learn to write code, I worked in the performing arts and am a big fan of all kinds of dance.)  List upcoming dance shows, no problem, right?  Well, the first website I tried to scrape wouldn't let me easily access the details of the HTML (they were all tagged with reactdata ids, which I don't have any experience with), so I started thinking that current listings might be tricky to scrape. 

I rethought my plan about my gem and decided to do something about hiking trails in the area instead.  I figured the websites dedicated to hiking would not be subject to the trouble that I was having scraping the calendar listings from the arts page.  What I didn't expect was to find that most of the sites I look at were written a long time ago and scraping them was proving tedious.  (div div div div div div div div div...)

I realized that I need a 'Goldilocks', not too cutting edge and not too outdated.  I decided to go back to my original gem idea about the Dance Listings and see if I could find the perfect website to scrape.  That's when I thought of the Dancer's Group website.  They have been an active organization in the San Francisco dance community for a long time and have a great calendar page that proved to be just perfect.  I was able to scrape the data and get my gem working!  

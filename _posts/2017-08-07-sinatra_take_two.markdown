---
layout: post
title:  "Sinatra, take two."
date:   2017-08-07 10:30:48 +0000
---


I just had my Sinatra project review with Luke, one of the very knowledgable instructors at Flatiron.  I was pretty confident going in.  I mean, I knew that my app rough around the edges but I thought that it would stand up to the tests of the assignment.  At the onset of the call we discussed Sinatra as a platform.  He shared with me his views on its merits and efficiency and wondered how I had liked working with it.  I parroted what I had heard from other students and some of the learn experts; namely that it was a good way to understand what is going on under the hood when working with Rails.  He had a surprising perspective that Sinatra was better tailored to his desires to build as efficiently as possible.  Not in the ease of the keystrokes that Rails provides, but with the streamlining that comes from building only what you need to in Sinatra.  Fair point, and one that I hadn't really considered.  

With that he was ready to take my app for a test drive.  He signed in and poked around at the home page of my Chore Wheel app for about 5 seconds and said something to the effect of "cool domain, what happens if I try to sign up again?".   I encouraged him to go right ahead, knowing that I had some presence validators in place to protect from whatever his nefarious plans were.  Then he proceeded to sign up agian with the SAME USERNAME!  I was speechless.  Why would he do something like that?  Of course, everything in the app went haywire.  Now there was repeated data in the DB and no queries would run.  Game over.  He was nice enough to point out a few other gaping holes in my use of cuurent_user valiadtions and the logged_in? helper method that can ensure user privacy protection (when implemented correctly).  

So, it's back to the drawing board for me.  I feel so naive.  And strangely empowered.  

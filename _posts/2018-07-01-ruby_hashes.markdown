---
layout: post
title:      "Ruby Hashes "
date:       2018-07-02 03:48:53 +0000
permalink:  ruby_hashes
---


I have decided to take a deep(ish) dive into hashes.  They are the most mysterious data-type, and also one of the most pwerful tools we have at our disposal.  I wanted to brush up on my understanding of how they work and also wrap my head around some of the sytactical sugar (like the &:[method] thing that has confounded my for months now!)  Seriously, what is that?  I had to know.  Read on and all will be revealed...


## What is a Hash?
A hash is a data structure that is comprised of a key and a value.  They are very useful for storing information about almost everything.  For example: a contact list has names and addresses or a wine list might have bottles and prices.  Anything that has a value associated with it is a good candidate to be stored in a hash.

## Creating  a Hash

Lets start at the very beginning...

There are a couple of options for creating a hash.

To simply declare the hash and then populate it:

```
wine_list = { "pinot noir" => 12, "sauvignon blanc" => 11 }
      
```

or use the Hash.new method to create the hash and then the bracket notation: ( [key] = value ) to assign keys and values :
   
```
wine_list = Hash.new
wine_list["pinot noir"] = 12
wine_list["sauvignon blanc] = 11

```

Ruby also provides the option of creating keys as symbols instead of strings.  The advantage of using symbols is that, unlike strings, they are immutable, meaning they always have the sam object id in memory and are therefore MUCH more useful in actual application.






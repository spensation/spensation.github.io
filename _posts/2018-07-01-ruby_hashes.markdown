---
layout: post
title:      "Ruby Hashes "
date:       2018-07-01 23:48:53 -0400
permalink:  ruby_hashes
---


I have decided to review hashes.  They are the most mysterious data-type, and also one of the most powerful tools we have at our disposal.  I wanted to brush up on my understanding of how they work and also wrap my head around some of the sytactical sugar (like the &:[method] thing that has confounded my for months now!)  Seriously, what is that?  I had to know.  Read on and all will be revealed...


## What is a Hash?
According to Ruby-Docs, 


> A Hash is a dictionary-like collection of unique keys and their values. Also called associative arrays, they are similar to Arrays, but where an Array uses integers as its index, a Hash allows you to use any object type.
> 
> Hashes enumerate their values in the order that the corresponding keys were inserted.


They are very useful for storing information about almost everything.  For example: a contact list has names and addresses or a wine list might have bottles and prices.  Anything that has a value associated with it is a good candidate to be stored in a hash.

## Creating  a Hash

Lets start at the very beginning...

There are a few of options for creating a hash.

To simply declare the hash and then populate it:

```
wine_list = { "pinot noir" => 12, "sauvignon blanc" => 11 }
      
```

or use the Hash.new method to create the hash and then the bracket notation: ( [key] = value ) to assign keys and values :
   
```
wine_list = Hash.new
wine_list["pinot noir"] = 12
wine_list["sauvignon blanc"] = 11

```

Ruby also provides the option of creating keys as symbols instead of strings.  The advantage of using symbols is that, unlike strings, they are immutable, meaning they always have the same object id in memory and are therefore MUCH more useful in application.

To create a hash using sybols as keys:

```
wine_list { :pinot_noir => 12, :sauvignon_blanc => 11 }
```

Or, you could write that as:

```
wine_list { pinot_noir: 12, sauvignon_blanc: 11 }
```

Hashes have a default value that is returned when accessing keys that do not exist.  If no default is explicitly set, nil is used.  To set a default value, send it into the ::new nethod as an argument:

```wine_list = Hash.new(0)
```

Or by using the default= method:

```
wine_list { pinot_noir: 12, sauvignon_blanc: 11 }
wine_list.default = 0
```
In order to access a value in a hash, use its key:

```
wine_list[:pinot_noir]  # => 12
```


## Updating a hash

Lets say you are using a hash to manage your wine list.  Eventually, you are going to have to update the price of a bottle.  Now what?

It's really easy with bracket notation:

```
# original prices
wine_list  => { "pinot noir" => 12, "sauvignon blanc" => 11 }

# update price  
wine_list["pinot noir"] = 14

# hash now returns new prices
wine_list  # => {"pinot noir" => 14, "sauvignon blanc" => 11}
```

## Iterating over a Hash


The **.each** method calls a block once for each key in a hash:

```
wine_list = { "pinot noir" => 14, "sauvignon blanc" => 11 }
wine_list.each { |wine, price| puts "The bottle of #{wine} is $#{price}" }
```
produces:

```
The bottle of pinot noir is $14
The bottle of sauvignon blanc is $11
```

Similarly, you can use **.each_key** or **.each_value** to access only the hash's keys or values, respectively.

## Manipulating the Hash

If you really want to get into all the options that exist, check out the Ruby-Docs on Hash class [https://ruby-doc.org/core-2.5.1/Hash.html](http://)https://ruby-doc.org/core-2.5.1/Hash.html

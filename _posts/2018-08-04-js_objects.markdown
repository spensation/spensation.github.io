---
layout: post
title:      "JS Objects"
date:       2018-08-04 07:07:09 +0000
permalink:  js_objects
---


I want to review my understanding of JavaScript Objects.  At the most basic level, an object must be defined as a variable:
```
const cat = {};
```

As cat is currently an empty object, should we call it in the console it would return this:

```
[object Object]
```

We can give this cat some personality by defining some 'members', or name/value pairs.

for example:

```
const cat = {
     name: 'Mittens',
		 age: 3,
		 breed: 'Maine Coon',
		 eyes: 'blue',
		 gender: 'female',
		 greeting: function() {
		     alert('Meow')
			}
};
```

Names and values are delineated by a colon and members are seperated by a comma.  Also notice that a function can be a member of an object.

Now we have some data to play with!

To access the data we two options, dot notation and bracket notation.  Dot notation is the more readable of the two but bracket notation is a powerful tool used to compute values.  It's functionality goes far beyond that of dot notation.

But I digress, here is some dot notation:

```
cat.name;

  *=> 'Mittens'*
	
cat.eyes;

  *=> 'blue'*
	
```

We can use dot notation to assign a new value to a name, as such:

```
cat.name = 'Fluffy';

 => "Fluffy"
 
```




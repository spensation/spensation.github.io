---
layout: post
title:      "JS Objects"
date:       2018-08-04 03:07:10 -0400
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
		 hobbies: ['sleeping', 'cuddling', 'hunting'],
		 meals: {
		    breakfast: 'kibble',
				dinner: 'tuna'
		 },
		 greeting: function() {
		     alert(this.name + 'says Meow!')
			}
};
```

As you can see, an object like this one,  a 'literal object', can be made up of nearly anything.  We have strings, integers, an array of strings, another object and a function packed in this little cat.  
Names and values are delineated by a colon and members are seperated by a comma. 


Now we have some data to play with!

To access the data we two options, dot notation and bracket notation.  Dot notation is the more readable of the two but bracket notation is a powerful tool used to compute values.  It's functionality goes far beyond that of dot notation.

**Dot Notation**

In order to access the value inside the object, first call the object itself(cat), then a dot(.), followed by the member name whose value you wish to retrieve.

```
cat.name;

  *=> 'Mittens'*
	
cat.hobbies[0];

  *=> 'sleeping'*
	
cat.meals.dinner;

*  => 'tuna'*
	
```

We can use dot notation to assign a new value to a name, as such:

```
cat.name = 'Fluffy';

 *=> "Fluffy"*
 
```


**Bracket Notation**

While bracket notation is less readable, it can be a good choice to usein select situations.

To simply access a value of the object:

```
cat['name'];

 * => 'Fluffly'*
	
cat['meals']['dinner'];

 * => 'tuna'*
```




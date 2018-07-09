---
layout: post
title:      "React/Redux Dynamic Array Form Fields"
date:       2018-07-09 03:59:42 +0000
permalink:  react_redux_dynamic_array_form_fields
---


In building my RecipeBook React App for my portfolio project, I made the choice to have all of my form inputs be very rudimentary.  My thinking was that I was going to have plenty to deal with in getting the React/Redux app bootstrapped, so I wanted to keep everything else as simple as possible.  In one way that strategy paid off: I, eventaully, finished my portfolio project.  After a few weeks of not banging my head against my machine, I dove back into my app.  I quickly realized that I needed to upgrade several things.  One of which was the way I was handling the ingredients of a recipe.  As written, a recipe's ingredients were simply a text-area of a form.  It proved difficult to format and manage.  I decided to give each ingredient in a recipe it's own form field and the UI would dictate how many fields were necessary.  What I envisioned was a dynamic array of ingredients.

Ok, so where to begin?

### The Backend:

My app is supported by a Rails API, so to get started on this feature, I started with my models.  Since, I new I was going to need a model for Ingredient AND the ability to create and destroy instances of this new model, I chose to create a new resource, Ingredient.

```
rails g resource Ingredient name:string 
```

I edited the new migration file to reflect the has_many/belongs_to association I needed Ingredient to have with Recipe, an established model:

```
class CreateIngredients < ActiveRecord::Migration[5.1]
  def change
    create_table :ingredients do |t|
      t.string :name
      t.belongs_to :recipe, index: true
      t.timestamps
    end
  end
end
```
Then in the ingredient.rb file:

```
class Ingredient < ApplicationRecord
	belongs_to :recipe
end
```
And finally, in recipe.rb:

```
class Recipe < ApplicationRecord
  // recipes have comments and likes :)
	has_many :comments
	has_many :likes
	// this is  association to Ingredients
	has_many :ingredients
end

```

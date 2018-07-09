---
layout: post
title:      "React Dynamic Array Form Fields,  Part I"
date:       2018-07-08 23:59:43 -0400
permalink:  react_redux_dynamic_array_form_fields
---


In building my RecipeBook React App for my portfolio project, I made the choice to have all of my form inputs be very rudimentary.  My thinking was that I was going to have plenty to deal with in getting the React/Redux app bootstrapped, so I wanted to keep everything else as simple as possible.  In one way that strategy paid off: I, eventaully, finished my portfolio project.  After a few weeks of not banging my head against my machine, I dove back into my app.  I quickly realized that I needed to upgrade several things.  One of which was the way I was handling the ingredients of a recipe.  As written, a recipe's ingredients were simply a text-area of a form.  It proved difficult to format and manage.  I decided to give each ingredient in a recipe it's own form field and the UI would dictate how many fields were necessary.  What I envisioned was a dynamic array of ingredients.

Ok, so where to begin?

### The Backend:

My app is supported by a Rails API, so to get started on this feature, I started with my models.  Since, I knew I was going to need a model for Ingredient AND the ability to create and destroy instances of this new model, I chose to create a new resource, Ingredient.

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

*ingredient.rb*
```
class Ingredient < ApplicationRecord
	belongs_to :recipe
end
```
And finally, in recipe.rb:

*recipe.rb*
```
class Recipe < ApplicationRecord
  // recipes have comments and likes :)
	has_many :comments
	has_many :likes
	// establish the association to Ingredients
	has_many :ingredients
end

```

After a quick check in the Rails console to see if I could create a new Ingredient that was associated with a recipe, I went on to build a controller for Ingredient:

*ingredients_controller.rb*
```
class IngredientsController < ApplicationController
	def create
	  // find the recipe that the ingredient will belong_to
		@recipe = Recipe.find(params[:recipe_id])
		
		// build the ingredient off of that recipe with strong params
		@ingredient = @recipe.ingredients.create(ingredient_params)
		
		// render the results to json for use by the react app
		render json: @ingredient
	end

	def destroy
	    @ingredient = Ingredient.find(params[:id])
	    if @ingredient.destroy
	        render json: :no_content, status: 202
	    else
	      render json: {}, status: 400
	    end
	end

	private
		def ingredient_params
	      params.require(:ingredient).permit(:name, :recipe_id)
	    end
end
```

The final piece off the backend puzzle was to change the strong params in my recipes_controller file to accept an array of ingredients instead of a single ingredient.

*recipes_controller.rb*
```
...

private
    def recipe_params
      params.require(:recipe).permit(:id, :title, :category, :serves, :prep_time, :cook_time, :total_time, :directions, :ingredients => [:name])
    end
		
.....


```

Note that the ingredients array is the final symbol passed into permit.  I couldn't get it to work otherwise.


###  On to the Frontend 


The UI - 

My vision was for the user to be presented with a button to 'ADD Ingredient' to the recipe.  Press the button and a form field appears that prompts the user to add input.  The user should be able to add as many ingredients as they want.  They also need the ability to delete a field, if necessary.

So in pseudo code the UI needs to:

				map the array of ingredients;
							// with each iteration do the following
							present an input that has:
										a text field
										a placeholder
										a value 
										a call to an onChange function  //  function needed
							 a button to delete the input field   // function needed

				 a button (outside the loop) to add an ingredient  // function needed

Ok, that seems pretty doable.  Map over the array, present an input, and write three functions to handle the changes.  Not too bad.  I mean, I'm glad I didn't try to do it while I was figuring out React/Redux for the first time but now I feel pretty confident that i can get this thing done!
 
 I know I have to map over the array, which means I need an array to begin with.
 
 At the top of my form, which is a container, BTW, I need to modify the state of the component to reflect that ingredients is now an array

NewRecipeFormPage.js
```
...

class NewRecipeFormPage extends Component {
  constructor(props) {
    super(props);

    this.state = {
      title: '',
      category: '',
      serves: '',
      ingredients: [],
      directions: '',
      prep_time: '',
      cook_time: '',
      total_time: ''
    }
  }
	...
	
```


Great, now we are ready to do some mapping.  Further down in the same file, where the form input for the ingredients looked something like this:

```
<div className="recipe-form-ingredients"> 
            <input
              placeholder="Ingredients"
              ref="ingredients"
              onChange={this.ingredientHandleOnChange.bind(this)} />
          </div>
```

...we are now going to map our new array, implementing the features outlined in the pseudo code:

```
{this.state.ingredients.map((ingredient, i) => (
			<div key={i}>
					 <input
								type="text"
								placeholder={`Ingredient #${i + 1} name`}
								value={ingredient || ''}
								onChange={this.handleIngredientsOnChange.bind(this, i)} />
						<button type="button" onClick={this.handleRemoveIngredient.bind(this, i)}>-</button>
				</div>
  ))}
  <button type="button" onClick={this.handleAddIngredient.bind(this)}>Add Ingredient</button>
          
```

And add the functions to handle the changes:

```
 handleIngredientsOnChange(i, event) {
    let ingredients = [...this.state.ingredients];
    ingredients[i] = event.target.value ;
    this.setState({ ingredients });
  };

  handleAddIngredient(){
    this.setState(prevState => ({
      ingredients: [...prevState.ingredients, '']
    }));
  }

  handleRemoveIngredient(i){
    let ingredients = [...this.state.ingredients];
    ingredients.splice(i, 1);
    this.setState({
      ingredients: ingredients
    });
  }
```



 
 
 
 
 
	   




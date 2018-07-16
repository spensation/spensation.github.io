---
layout: post
title:      "React/Redux Dynamic Array Form Fields Part II: The Frontend"
date:       2018-07-16 01:09:47 -0400
permalink:  react_redux_dynamic_array_form_fields_part_ii_the_frontend
---


In my [last blog post](http://http://kspencerevans.com/react_redux_dynamic_array_form_fields), I dealt with the backend set up for handling dynamic array form fields for a React/Redux project with a Rails API.  This post will continue where I left on and focus on the frontend.  

## Creating The UI:

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
 handleIngredientsOnChange(name, event) {
    let ingredients = {};
    ingredients[name] = event.target.value ;
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



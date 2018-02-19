---
layout: post
title:      "Redux - Why we use Functional Components"
date:       2018-02-19 21:05:57 +0000
permalink:  redux_-_why_we_use_functional_components
---


So you think you have a handle on React?  Enter Redux...

I was feeling pretty confident building components, assigning State and Props and using setState to update data in my React assignments.  Then came Redux.  

Updating State from multiple components can cause problems and we want to use stateless components as much as possible.  Stateless components are also referred to as Functional,  Presentational or  'Dumb' components because they are not concerned with State.  

for example:

```
RecipeListItem.js...

import React from 'react';
import { connect } from 'react-redux';
import { bindActionCreators } from 'redux';
import { fetchRecipe } from '../actions/recipes.js';

const RecipeListItem = (props) => {
  return (
  <div
    className="recipe-card"
    onClick={() => {
                  props.fetchRecipe(props.recipeId);
                  props.history.push(`/recipes/${props.recipeId}`)
                }}>
    <p className="recipe-title">{props.recipeTitle}</p>
    <p className="recipe-category">{props.recipeCategory}</p>
    <p className="recipe-serves">Yields: {props.recipeServes}</p>
  </div>
  )
}

function mapDispatchToProps(dispatch) {
  return {fetchRecipe: bindActionCreators(fetchRecipe, dispatch)}
}

export const WrapperRecipeListItem = connect(null, mapDispatchToProps)(RecipeListItem);

```

This snippet is from my recipe sharing project.  Let's try to break down what is happening and examine why we can acheive our goals with a stateless component.

The RecipeListItem is responsible for initiaing a call to the API and bringing back a truncated recipe listing.  I want to use these listings to fill individual recipe cards on the main page of my app.  Ultimately, the card can be clicked and we will go to the 'show-page'.  (I use quotes because in a SPA (single-page-application), we aren't really going anywhere, we are simply revealing, or hiding, components.

The way I see it, this component has three parts:  dependencies, content, and functionality.


DEPENDENCIES:

```
import React from 'react';
import { connect } from 'react-redux';
import { bindActionCreators } from 'redux';
import { fetchRecipe } from '../actions/recipes.js';
```

This is pretty straight-forward.  In order for React Components to communicate with each other, you need to connect them.  RecipeListItem.js depends on these other files and functions to operate.  One tricky thing is the curly bracket notation seen above.  The difference between import React from 'react'  and the other three dependencies above is that connect, bindActionCreators and fetchRecipe are functions or variables found within the files they are being extracted from and have been explicitly exported as named variables.


COMPONENT CONTENT:

```
const RecipeListItem = (props) => {
  return (
  <div
    className="recipe-card"
    onClick={() => {
                  props.fetchRecipe(props.recipeId);
                  props.history.push(`/recipes/${props.recipeId}`)
                }}>
    <p className="recipecard-title">{props.recipeTitle}</p>
    <p className="recipecard-category">{props.recipeCategory}</p>
    <p className="recipecard-serves">Yields: {props.recipeServes}</p>
		<p classname="recipecard-total-time">{props.recipeTotalTime}</p>
  </div>
  )
}
```

I define the const RecipeListItem and pass it props.  It consists of a div of class 'recipe-card' with an OnClick property that triggers the fetch request action and also pushes the props.history  to the route '/recipes/${props.recipeId}'.  The click result is to send the user to the 'Show Page' of the app, where the entire recipe will be displayed.

The RecipeListItem div is populated with several props of the recipe: title, category, serves, and total_time.  The idea is to give the user some critical information about the recipe before they commit to a complete view.  


FUNCTIONALITY:

Now for the great mystery:  How do we make this happen, exactly?

The short answer is this:  Through the magic of mapDispatchToProps and Connect.

Let's tease it out a bit...

```
function mapDispatchToProps(dispatch) {
  return {fetchRecipe: bindActionCreators(fetchRecipe, dispatch)}
}

export default connect(null, mapDispatchToProps)(RecipeListItem);

```

What is going on here?  I define a function, mapDispatchToProps(),  that takes dispatch as an argument and returns the action that I need called on my component.  In this case, fetchRecipe().  I also use bindActionCreators, which gives my functional component access to dispatch directly.  this function results in a call to recipes.js, which sits in my actions folder.  This is where all the actions associated with recipes live in my app. Once that fetchRecipe action is called, my recipe_recducer will look for a matching dispatch type.  If it finds one, a request will fire.

Then I export the file in order for it to be picked up by RecipeList.js and  rendered to the page:

```
RecipeList.js...

import React from 'react';
import { connect } from 'react-redux';
import { bindActionCreators } from 'redux';
import * as actions from '../actions/recipes.js';
import RecipeListItem from './RecipeListItem';



const RecipeList = (props) => {
  const renderRecipes = props.recipes.map(recipe =>
    <RecipeListItem
      history={props.history}
      key={recipe.id}
      recipeTitle={recipe.title}
      recipeCategory={recipe.category}
      recipeServes={recipe.serves}
      recipeTotalTime={recipe.total_time}
      recipeId={recipe.id}
    />
  );

  return (
    <div>
      {renderRecipes}
    </div>
  )
}

function mapStateToProps(state) {
  return {recipes: state.recipes.recipes}
}

function mapDispatchToProps(dispatch) {
  return {actions: bindActionCreators(actions, dispatch)}
}

export default connect(mapStateToProps, mapDispatchToProps)(RecipeList);

```


In conclusion, Redux is not simple.  It is, however, very powerful and it does a brilliant job at seperating concerns.  Under Redux, each React component is responsible for it's share of the workload and is blissfully unaware of the state of the application.  all state is in the store and will be updated when the actions and reducers work in tandem to allow it to happen.  Functional Components will never know about it, and that's just the way it should be.








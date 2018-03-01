---
layout: post
title:      "Redux - How to Use Functional Components"
date:       2018-02-19 16:05:58 -0500
permalink:  redux_-_why_we_use_functional_components
---


So you think you have a handle on React?  Enter Redux...

I was feeling pretty confident building components, assigning State and Props and using setState to update data in my React assignments.  Then came Redux.  

Updating State from multiple components can cause problems and we want to use stateless components as much as possible.  Stateless components are also referred to as Functional,  Presentational or  'Dumb' components because they are not concerned with State.  

for example:

```
import React from 'react';
import { Link } from 'react-router-dom';


const RecipeGrid = (props) => {
  const renderRecipes = props.recipes.recipes.map(recipe =>
    <div className="recipe-card">
      <h4>{recipe.title}</h4>
      <p>{recipe.category}</p>
      <p>Yields: {recipe.serves}</p>
      <Link className="view-recipe-link" to={`/recipes/${recipe.id}`}>View Recipe</Link>
    </div>
  );

  return (
    <div>
      {renderRecipes}
    </div>
  )
}

export default RecipeGrid;

```

This snippet is from my recipe sharing project.  Let's try to break down what is happening and examine why we can acheive our goals with a stateless component.

The RecipeGrid is responsible for the presentation of the array of recipes.  It wants to display only the essential information about the recipe to the user.  Here the array of recipes is mapped and each recipe presents its title, category and serves attributes.  

The RecipeGrid is the child of the RecipeGridPage.  This parent component is responsible for the functionality of the RecipeGrid, actually getting the data to the page.  It is connected to the store, Redux's state management processor, and can dispatch State and Dispatch to Props so that the props and actions of a recipe can be passed around throughout the application.  

```
import React from 'react';

import { connect } from 'react-redux';
import { bindActionCreators } from 'redux';

import { fetchRecipes } from '../actions/recipes';
import RecipeGrid from '../components/RecipeGrid';


class RecipeGridPage extends React.Component {
  
  componentDidMount() {
      return this.props.fetchRecipes();
  }

  render(){
    return (
      <div>
        <RecipeGrid recipes={this.props.recipes} history={this.props.history}/>      
      </div>
    )
  }
}

const mapStateToProps = (state) => {
  return { recipes: state.recipes };
}

const mapDispatchToProps = (dispatch) => {
  return bindActionCreators({
    fetchRecipes: fetchRecipes,
  }, dispatch);
};

export default connect(mapStateToProps, mapDispatchToProps)(RecipeGridPage);

```

So, the parent component, RecipeGridPage, needs the dependencies to connect to the store. 

```
import { connect } from 'react-redux';
import { bindActionCreators } from 'redux';
```

These allow us to utilize the following lines of code that are the lynchpin that make React so powerful:

```
const mapStateToProps = (state) => {
  return { recipes: state.recipes };
}

const mapDispatchToProps = (dispatch) => {
  return bindActionCreators({
    fetchRecipes: fetchRecipes,
  }, dispatch);
};

export default connect(mapStateToProps, mapDispatchToProps)(RecipeGridPage);
```

by mapping state to props and dispatch to props, React allows us to call functions on these elements throughout the application.  


The other element of this parent component that is critical to making the stateless RecipeGrid component work is the ability to directly pass props to the child component as here, in the render call:

```
render(){
    return (
      <div>
        <RecipeGrid recipes={this.props.recipes} history={this.props.history}/>      
      </div>
    )
  }
```
By working together with thier parent components. functional, or stateless components are an efficient tool to bring execptional user experiences to the page.








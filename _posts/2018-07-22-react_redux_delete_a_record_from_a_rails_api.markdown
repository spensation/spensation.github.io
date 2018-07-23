---
layout: post
title:      "React/Redux: Delete a Record from a Rails API"
date:       2018-07-22 21:59:09 -0400
permalink:  react_redux_delete_a_record_from_a_rails_api
---


In order to hone my newly acquired React/Redux skills, I have been working on a new project: The Nap App.  I need a way to track my son's napping schedule, so naturally, I started building an app!  Things were going along swimmingly.  I built a new rails API to power the backend, I ran create-react-app and started to dig into the frontend.  THis was going to be a piece of cake!  


Well, I had an pretty easy time getting the app to the point where I could create an record of a nap, post it to the api and then view the index of naps.  I decided to go a step further... I want to be able to delete a nap.


**Building the UI:**

In order to delete the nap, we need the User to be able to make that choice.  We will add a 'Delete' button to the Nap Component:

```
import React from 'react';

const Nap = (props) => {
	return(
	<div className="nap">
		<h3>{props.children} </h3>
		<p>{props.description}</p>
     // Add delete button
    <button>Delete</button>
	</div>
		)
}

export default Nap;

```

Because the function that we will build to handle this delete event needs to deal with state (or the Redux store, in this case) we will need to pass the functionality of the onClick attribute onto a stateful Container.  That was a mouthful!  It takes a little while to wrap your head around this idea, but in practice, it's pretty straightforward: we can do it with props!

*from Nap.js*
```
...
// use props to pass onDelete to the Container
<button onClick={props.onDelete}>Delete</button>

...

```


**Handling the event:**

Over in our Container, to each instance of Nap,  we will pass the onClick event the props of onDelete  and set it equal to a function we will define named  deleteThisNap().

*from NapPage.js*
```
...

render() {
    console.log('InNapPage', this)
    return (
      <div className="container">
        <div className="header">Track thier naps</div>
        <div className="nap">
          {
            this.props.naps.map((nap, index) => {
              return(<Nap 
                key={nap.id}
                description={nap.description}
								
								// pass props from onClick event in Nap.js 
								
                onDelete={deleteThisNap()}
                >{nap.name}</Nap>)
            })
          }
        </div>


      </div>
    );
  }
	
	...
	
```

In the same container file, let's define our function.  There are a couple of things to keep in mind here.  First, because it is an event handler, we need to pass an event  to the function.  This will allow us to bind the function to this event, or button click.  We also need to hit the API route to delete the nap, which will require a specific nap_id.  In order to accomplish this, we can use the index of the map function to define a variable called napId that we will then pass to the function that dispatches the DELETE action. 


*from NapPage.js*
```
...

class NapPage extends Component {



  constructor(props) {
    super(props);
    this.state = {
      naps: [],
    }

  }

  componentDidMount() {
    return this.props.fetchNaps()
  }

  
  deleteThisNap = (index, e) => {
	  // define variable used to to pass id to the api
    const napId = this.props.naps[index].id;
		// pass napId to the function that dispatches the action to the Redux store
    this.props.deleteNap(napId)  
  }

...
...


<div className="nap">
          {
            this.props.naps.map((nap, index) => {
              return(<Nap 
                key={nap.id}
                description={nap.description}
								
								// updated onDelete prop with binding and args
								
                onDelete={this.deleteThisNap.bind(this, index)}
                >{nap.name}</Nap>)
            })
          }
        </div>

...
```

**Dispatching the Action:**

In the Actions folder, we have a file called naps.js.  This is where the action is!  Since we are already able to fetch and add naps, we know this file is functioning.  We simply need to extend its functionality to include deleting a nap.   (Note that the api calls are running locally to port 3001.  In production, these urls will have to reflect the host server)

*from actions/naps.js*
```
import fetch from 'isomorphic-fetch';

export function fetchNaps() {
  return (dispatch) => {
    dispatch({ type: 'LOADING_NAPS' });
    return fetch('http://localhost:3001/api/v1/naps', {
    	method: 'GET',
      	headers:{ Accept: "application/json",
               "Content-Type": "application/json"}
    }).then(response => response.json())
    .then(naps => dispatch({ type: 'FETCH_NAPS', payload: naps}))
  };
}

export function addNap(nap) {
  console.log('from addNap', nap)
  return (dispatch) => {
    dispatch({ type: 'ADD_NAP_PENDING' });

    return fetch('http://localhost:3001/api/v1/naps', {

      method: 'POST',
      body: JSON.stringify({ nap: nap }),
      headers:{ 'Accept': "application/json",
               "Content-Type": "application/json"}
    }).then(response => response.json())
      .then(nap => dispatch({ type: 'ADD_NAP_FULFILLED', payload: nap }))
    };
  }

// Add deleteNap function

export function deleteNap(napId) {
    console.log('inDeleteNapACTION', napId)
    return dispatch => {
      dispatch({type: 'DELETE_NAP_PENDING'});
      return fetch(`http://localhost:3001/api/v1/naps/${napId}`, {
          method: "DELETE",
      }).then(response => {
			// Upon dispatching DELETE action, set 'deletedNapId' to napId.  This will be used in the      reducer to filter state
        if (response.ok) dispatch({ type: 'DELETE_NAP_FULFILLED', deletedNapId: napId })
      }) 
    };
  };
```
Since we have now defined our deleteNap action, let's revisit our container and have a look at the entire file:

*from containers/NapPage.js*
```
import React, { Component } from 'react';
import '../App.css';
import Nap from '../components/Nap';
import { connect } from 'react-redux';
import { bindActionCreators } from 'redux';
import { fetchNaps } from '../actions/naps';
import { addNap } from '../actions/naps';
// import deletNap function from actions/naps.js file
import { deleteNap } from '../actions/naps';

class NapPage extends Component {


  constructor(props) {
    super(props);
    this.state = {
      naps: [],
    }

  }

  componentDidMount() {
    return this.props.fetchNaps()
  }

  
  deleteThisNap = (index, e) => {
    const napId = this.props.naps[index].id;
    this.props.deleteNap(napId)  
  }
 

  render() {
    console.log('InNapPage', this)
    return (
      <div className="container">
        <div className="header">Track thier naps</div>
        <div className="nap">
          {
            this.props.naps.map((nap, index) => {
              return(<Nap 
                key={nap.id}
                description={nap.description}
                onDelete={this.deleteThisNap.bind(this, index)}
                >{nap.name}</Nap>)
            })
          }
        </div>


      </div>
    );
  }
}

const mapStateToProps = (state) => {
  return { 
    naps: state.naps.naps,
  };
}

const mapDispatchToProps = (dispatch) => {
  return bindActionCreators({
    fetchNaps: fetchNaps,
    addNap: addNap,
		
		// add deletNap action to mapDispatchToProps
    deleteNap: deleteNap
  }, dispatch);
}

export default connect(mapStateToProps, mapDispatchToProps)(NapPage);
```


**En fin, the reducer**

At last, we arrive at the final piece of the puzzle: naps_reducer.js.  Like actions/naps.js, this file is already functioning to fetch and add naps.  We will simply extend the case statement to handle deleting a record of our database.  

*from reducers/naps_reducer.js*
```
export function napReducer(state = {
  isFetching: false,
  posting: false,
  naps: [],
  nap: {},
  
}, action) {
  switch(action.type) {
    case 'LOADING_NAPS':
      return {
        ...state,
        isFetching: true
      }

    case 'FETCH_NAPS':
      console.log('inFetch_naps', action.payload)
      return {
        naps: action.payload,
          isFetching: false
          
        }
 
    case 'ADD_NAP_PENDING':
      return {...state, posting: true}
    case 'ADD_NAP_FULFILLED':
      return Object.assign({}, state, {nap: state.naps.concat(action.payload) });
    
		// Handle deletion here
    case 'DELETE_NAP_FULFILLED':
		// Define variable to filter state by comparing nap.id to action.payload 
      const naps = state.naps.filter(nap => nap.id !== parseInt(action.deletedNapId, 10));
      return { naps };
    default:
      return state;

  }
}

```

And that's it.  Simple, right?





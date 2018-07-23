---
layout: post
title:      "React/Redux: Delete a Record from a Rails API, Part I"
date:       2018-07-22 21:59:09 -0400
permalink:  react_redux_delete_a_record_from_a_rails_api
---


In order to hone my newly acquired React/Redux skills, I have been working on a new project: The Nap App.  I need a way to track my son's napping schedule, so naturally, I started building an app!  Things were going along swimmingly.  I built a new rails API to power the backend, I ran create-react-app and started to dig into the frontend.  THis was going to be a piece of cake!  


Well, I had an pretty easy time getting the app to the point where I could create an record of a nap, post it to the api and then view the index of naps.  I decided to go a step further... I want to be able to delete a nap.


## Building the UI:

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

Because the function that we will build to handle this delete event needs to deal with state (or the Redux store, in this case) we will need to pass the functionality onClick attribute onto a stateful Container.  That was a mouthful!  It takes a little while to wrap your head around it, but in practice, it's pretty straightforward: we can do it with props!

*from Nap.js*
```
...
// use props to pass onDelete to the Container
<button onClick={props.onDelete}>Delete</button>

...

```


## Handling the event:

Over in our Container we will pass the onClick event props of onDelete to each instance of Nap and set it equal to a function we will define named  deleteThisNap().

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

In the same container file, let's define our function.  There a re a couple of things to keep in mind here.  First, because it is an event handler, we need to pass an event  to the function.  This will allow us to bind the function to this event, or button click.  We also need to hit the API route to delete the nap, which will require a specific nap_id.  In order to acomplish this, we can use the index of the map function to define a variable called napId that we will then pass to the function that dispatches the DELETE action. 


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

#### Dispatching the Action:










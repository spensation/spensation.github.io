---
layout: post
title:      "React/Redux: Delete a Record from a Rails API"
date:       2018-07-23 01:59:07 +0000
permalink:  react_redux_delete_a_record_from_a_rails_api
---


In order to hone my newly acquired React/Redux skills, I have been working on a new project: The Nap App.  I need a way to track my son's napping schedule, so naturally, I started building an app!  Things were going along swimmingly.  I built a new rails API to power the backend, I ran create-react-app and started to dig into the frontend.  THis was going to be a piece of cake!  


Well, I had an pretty easy time getting the app to the point where I could create an record of a nap, post it to the api and then view the index of naps.  I decided to go a step further... I want to be able to delete a nap.


##### Building the UI:

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


#### Handling the event:

Over in our Container we will pass the onClick event props of onDelete to each instance of Nap.

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
								
                onDelete={this.deleteNap.bind(this, index)}
                >{nap.name}</Nap>)
            })
          }
        </div>


      </div>
    );
  }
	
	...
	
```








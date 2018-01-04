---
layout: post
title:      "Manipulating the DOM"
date:       2018-01-04 00:44:11 +0000
permalink:  manipulating_the_dom
---


It is both so simple and so complicated, this jQuery frontend business.  In essence, the task is to present the user with the data they are after in the most efficient manner possible.  Well, that is until someone comes up with whatever will replace AJAX requests.  Why is this efficiency so complicated to execute?  This has been the burning question that has brought me to this point in my journey to become a developer.  

First of all, you have to have your project functioning as an old school HTTP request came of catch.  If you can't make it work in the controller alone, you have no chance of making it work with jQuery/AJAX.    And thus, armed with a pretty functional project, I waded into the waters of the river AJAX.   Little did I know how trecherous those waters can become.  

After pouring over the readings and labs a few times I took on the first neccessary task, hijacking the event.  It actually went great on the first target.  Following the todoMVC model, I had a task list that the user can add tasks to by entering the data into a form field and pressing enter.   I successfully intercepted the creation event by writing the following lines of code in my tasks.js file:

```
$(function(){
  $("#new_task").on("submit", function(e){
    e.preventDefault()
		alert('Yo!')
		}
 }
```

Now when I tried to create a new task, instead of going through with the HTTP request, I got a pop-up alert saying 'Yo!'.  Now I had to create that task using AJAX.

The jQuery method $.ajax allows you to define the parameters of the request you are building. I knew I wanted a POST request that created json out of the data that the user input to the form.  Armed with this knowledge, I started building an AJAX requst that looked like this:

```
   $.ajax({
      url: action,
      data: params,
      dataType: "json",
      method: "POST"
    })

```

In order for this to work, I needed to define a couple of  variables, 'action' and 'params'.

I started by defining a const for the form itself:

const $form = $(this)

then I could define the other variables in relation to "$form" : 

```
    const action = $form.attr("action");
    const params = $form.serialize();
```


I placed my newly defined varialbles where my alert had been: 

```$(function(){
    $("#new_task").on("submit", function(e){
      e.preventDefault()
      const $form = $(this);
      const action = $form.attr("action");
      const params = $form.serialize();

     $.ajax({
        url: action,
        data: params,
        dataType: "json",
        method: "POST"
      })
		 }
		}

```


I was on the precipice of manipulating the DOM!

Now, how do I do that again?

In this instance, I chose to use HandleBars, even though as I understand it, I may never look at HandleBars again after I learn React.  By invoking the prototype functionality native to JavaScript, I could I could create an instance of a task that would be populated by the data placed in a task-template that I would have to generate in my house_chores (the parent of my task list) view page.
The template looks like this:

```
<script id="task-template" type="text/x-handlebars-template">
  <li class="task" id="task_{{id}}">
    <div class="view">
      <label>{{description}}</label>
      <form class="button_to" method="post" action="/house_chores/<%= @house_chore.id %>/tasks/{{id}}">
        <input type="hidden" name="_method" value="delete">
        <input class="destroy" type="submit" value="x">
        <%= hidden_field_tag :authenticity_token, form_authenticity_token %>
      </form>
    </div>
  </li>
</script>

```

I wanted to append my target ul with a new li that contained my newly minted task.   In a perfect world I would have a success callback function that read something like this:

```
.success(function(json) {
  $('ul.todo-list').append(taskLI);
	})
```
In order to have this available to me, I needed to do some legwork at the top of my tasks.js file.

First, I had to write as function to establish task as a prototype:

```
function Task(attributes) {
  this.description = attributes.description;
  this.id = attributes.id;
}
```
Then I had to delineate and compile my HandleBars function:

``
$(function() {
  Task.templateSource = $('#task-template').html()
  Task.template = Handlebars.compile(Task.templateSource);
})`

```

Finally, I had to create a function that exectuted the rendering of the template on the prototype of Task:

```
Task.prototype.renderLi = function() {
  return Task.template(this)
}
```

With all that in place, I was ready to actually place my new task into the DOM via AJAX.  This was the moment I had been working towards for more hours than I care to admit.

Back in my .success callback function, I first cleared out the form field (for UX purposes) and then defined two more variables that I needed to complete the appending.  One to call a new instance of the Task prototype with the json data passed in  and the next to call the rendering function on that new instance of task:

```
.success(function(json) {
      $('#task_description').val("");
      var task = new Task(json);
      var taskLi = task.renderLi()


      $('ul.todo-list').append(taskLi);
    })
```

Boom.  New task in the DOM.  

If only all jQuery implementation could be this straightforward!!




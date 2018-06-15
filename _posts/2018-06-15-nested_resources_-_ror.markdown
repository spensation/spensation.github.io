---
layout: post
title:      "Nested Resources - RoR"
date:       2018-06-15 23:07:31 +0000
permalink:  nested_resources_-_ror
---


I'm making a very simple app to try to help my wife and I keep track of our son's napping habits.  This could easily turn into a blog post about the perils of new parenthood and sleep deprivation but I will do my best to keep this technical.

The idea of this app is that there are two resources: Days and Rests.  A Day has_many Rests and a Rest belongs_to a Day.  

I used:

```
rails generate resource Day date:date
```

and 

```
rails generate resource Rest name:string duration:integer
```

to generate my resources.  Day has an attribute called date that is of type date and Rest has attributes called name and duration of types string and integer, respectfully.


## Wiring up the models

In the models directory, I modified day.rb and rest.rb in order to wire up the associations as such:

...models/day.rb
```
class Day < ApplicationRecord
	has_many :rests
	accepts_nested_attributes_for :rests
	
end
```

...models/rest.rb
```
class Rest < ApplicationRecord
	belongs_to :day

end
```

The key here is the accepts_nested_attributes_for tag.  This will allow me to use the build function in order to create rests beneath a given day.

## Routing

After nesting my resource in my routes file, like this:

../config/routes.rb
```
...

resources :days do 
  	resources :rests
  end
	
	....
	
```
my routes will be as follows:


```
 day_rests         GET           /days/:day_id/rests(.:format)               rests#index
                               POST       /days/:day_id/rests(.:format)               rests#create
 new_day_rest GET           /days/:day_id/rests/new(.:format)     rests#new
edit_day_rest   GET          /days/:day_id/rests/:id/edit(.:format) rests#edit
     day_rest        GET          /days/:day_id/rests/:id(.:format)          rests#show
                               PATCH    /days/:day_id/rests/:id(.:format)           rests#update
                               PUT         /days/:day_id/rests/:id(.:format)           rests#update
                               DELETE /days/:day_id/rests/:id(.:format)           rests#destroy
         days            GET         /days(.:format)                                             days#index
                               POST      /days(.:format)                                            days#create
      new_day       GET         /days/new(.:format)                                   days#new
     edit_day         GET         /days/:id/edit(.:format)                            days#edit
          day              GET         /days/:id(.:format)                                     days#show
                                PATCH   /days/:id(.:format)                                     days#update
                                PUT          /days/:id(.:format)                                   days#update
                                DELETE /days/:id(.:format)                                    days#destroy
```


## Using build in the Controller

Over in my days controller, my #new method is defined as follows:

../controllers/days_controller.rb
```
...

def new
		@day = Day.new
		@day.rests.build
	end
	
	....
	
	
```

Because the nested resource is established in my routing and models, I am able to utilize this very convenient pattern.  The final peice of the puzzle is also here in the days_controller.

```

...

def create
		@day = Day.create(day_params)
		redirect_to day_path(@day)
	end

	def show
		@day = Day.find(params[:id])
	end


	private

		def day_params
			params.require(:day).permit(:date, rests_attributes: [:id, :name, :duration])
		end
		
		....
```

My #create method utilizes strong params, day_params, defined as a private method later in the controller.  By passing rests_attributes: [:id, :name, :duration] into the permit method of day_params, I am allowing the instance of day to be create with it's rests.  That's just what a really, really tired dad needs! 


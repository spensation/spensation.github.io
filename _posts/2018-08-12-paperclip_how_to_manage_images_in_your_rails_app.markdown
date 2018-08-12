---
layout: post
title:      "Paperclip: How to manage images in your rails app"
date:       2018-08-12 17:42:57 +0000
permalink:  paperclip_how_to_manage_images_in_your_rails_app
---


In an effort to level up my basic recipe sharing CRUD app, I want to have the user be able to upload an image of the dish that the are serving up.  Well, getting images to the page is no problem but managing image files on the backend can be a real headache.  Luckily, there is a gem to help with the process. Paperclip is a ruby gem that promises to manage the storage and retrieval of your image files in a rails application.  

First thing first, install the gem.  In your gemfile:


```
gem 'paperclip'
```

then install the gem from the command line:

```
bundle install
```

Now that we have our gem installed, we can generate a migration via the gem itself.   In this instance, I want to create a migration that will add an image file, that I will call dish_image, to the class Recipe.

again from the command line:

```
rails generate paperclip Recipe dish_image
```

This will create a migration that looks like this:

```
class AddAttachmentDishImageToRecipes < ActiveRecord::Migration[5.1]
  def self.up
    change_table :recipes do |t|
      t.attachment :dish_image
    end
  end

  def self.down
    remove_attachment :recipes, :dish_image
  end
end
```

Next, I will have to update the recipe_params in the recipe_controller.rb file to allow for our new file attachment upon creating a new recipe.

*recipe_controller.rb*
```
 private
    def recipe_params
      params.require(:recipe).permit(:id, :title, :category, :serves, :prep_time, :cook_time, :total_time,    :directions, :dish_image, {:ingredients_attributes => [:name, :recipe_id]})
    end
```

Finally, I need to update my Recipe.rb file in order to assiciate the attached file to the class Recipe and set some basic styling parameters , as such: 

```
class Recipe < ApplicationRecord
	has_many :comments
	has_many :likes
	has_many :ingredients
	has_attached_file :dish_image, styles: { medium: "300x300>", thumb: "100x100>" }
	accepts_nested_attributes_for :ingredients
end
```

And there you have it!  We can now upload image files to our rails app!  Next up, designing the UI to uplaod and display these images in a React framework.  Stay tuned!

---
layout: post
title:      "Rspec BDD for models"
date:       2018-08-17 23:55:56 -0400
permalink:  rspec_bdd_for_models
---


This week I am stepping back from the world of React/Redux and digging in to a new rails project: a wine cellar database.  Actually, I wrote about parts of a wine cellar api a couple weeks ago, thinking that I would build another app with a React/Redux frontend and a Rails api backend but I want to get back to pure rails and strengthen some of my foundational understanding of how to write quality code. 

Towards that end, lets talk about writing Rspec tests for models!  This project has a bunch of models but for this article, I want to focus on three of them: Countries, Producers and Appellations.  Associatively speaking, the models lookl like this:

*from models/country.rb*
```
class Country < ApplicationRecord
	has_many :appellations
	has_many :producers, through: :appellations
end
```

*from models/producer.rb*
```
class Producer < ApplicationRecord
	has_many :bottles
	belongs_to :appellation

	delegate :country, :to => :appellation, :allow_nil => true
	# allows producer.appellation_country
end
```

*from models/appellation.rb*
```
class Appellation < ApplicationRecord
	has_many :producers
	belongs_to :country
end
```

And the schema for these models looks like this:

*from db/schema.rb*
```
create_table "appellations", force: :cascade do |t|
    t.string "name"
    t.string "tier"
    t.string "region"
    t.integer "country_id"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
  end

  create_table "countries", force: :cascade do |t|
    t.string "name"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
  end

  create_table "producers", force: :cascade do |t|
    t.string "name"
    t.date "established"
		t.integer "appellation_id"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false 
  end
```

Notice that the appellations table has a column for country_id and the producers table has an appellation_id, reflecting that an appellation belongs_to a country and a producer belongs to an appellation.

Now, we want to write tests that verify that these associations hold true.



## Getting started with Rspec

The first thing to do is add rpsec-rails to your gemfile.  (you can skip this step if you already have Rspec installed.  If you already have a spec directory in your file tree, you are good to go and can skip down to writing the tests)

*from Gemfile*
```
...
group :development, :test do
  # Call 'byebug' anywhere in the code to stop execution and get a debugger console
  gem 'byebug', platforms: [:mri, :mingw, :x64_mingw]
  # Adds support for Capybara system testing and selenium driver
  gem 'capybara', '~> 2.13'
  gem 'selenium-webdriver'
  # Add rspec-rails in order to ensure TDD/BDD
  gem 'rspec-rails', '~> 3.7'
end
...

```

then run bundle install

The next step is to generate a spec directory. 

```
rails generate rspec:install
```
 This generates the spec directory and a rails_helper.rb file and a spec_helper.rb file.  
 
 
 ## Writing the tests
 
 

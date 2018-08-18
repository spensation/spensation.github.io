---
layout: post
title:      "Rspec BDD for models"
date:       2018-08-17 23:55:56 -0400
permalink:  rspec_bdd_for_models
---


This week I am stepping back from the world of React/Redux and digging in to a new rails project: a wine cellar database.  Actually, I wrote about parts of a wine cellar api a couple weeks ago, thinking that I would build another app with a React/Redux frontend and a Rails api backend but I want to get back to pure rails and strengthen some of my foundational understanding of how to write quality code. 

Towards that end, lets talk about writing Rspec tests for models!  This project has a bunch of models but for this article, I want to focus on three of them: Countries, Producers and Appellations.  Associatively speaking, the models look like this:

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

The first thing to do is add rpsec-rails to your gemfile.  (you can skip this step if you already have Rspec installed.  If you already have a spec directory in your file tree, you are good to go and can skip down to formatting .rspec)

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
 
 
 ## Formatting .rspec
 There are a couple things to do in the .rspec file in order to optomize our Rspec experience.
 add these three lines under --require spec helper
 
 *.rspec*

```
 --require spec helper
--require rails_helper
--color
--format documentation
```

spec_helper is for Rspec versions < 3
rails_helper is for versions >= 3
color makes it green/red
format documentation logs the results of the tests to the command line


 ## Writing the tests
 
 First things first, we need to create a models directory to hold all of our model specs.
 
 cd to the spec directory and run
 
```
mkdir models
```

Now we can create files for our three specs.
 
Lets start with the country model.  A country has a name,  has_many appellations and has_many producers through appellations.  When we write the Rspec test, we will simply describe these requirements and associations by creating instances of the associated models and defining the expectations of their existence.  That was a mouthful!  It should become clear in one minute. 

*spec/models/country_spec.rb*
```
require 'rails_helper'

describe Country do
	
	it 'has a name' do
		expect { Country.create(:name => 'Italy') }.to_not raise_error
		france = Country.create(:name => 'France')
		expect(france.name).to eq('France')
	end

	it 'has many appellations' do
		france = Country.create(:name => 'France')
		languedoc = Appellation.create(
			:name => 'Languedoc',
			:tier => 'AOC',
			:region => 'Languedoc-Rousillion',
			:country_id => france.id
			)
		cotes_du_rhone = Appellation.create(
			:name => 'Cotes du Rhone',
			:tier => 'AOC',
			:region => 'Rhone',
			:country_id => france.id
			)

		updated_france = Country.find_by(:name => 'France')

		expect(updated_france.appellations).to include(languedoc)
		expect(updated_france.appellations).to include(cotes_du_rhone)
	end

	it 'has many producers through appellations' do
		france = Country.create(:name => 'France')
		languedoc = Appellation.create(
			:name => 'Languedoc',
			:tier => 'AOC',
			:region => 'Languedoc-Rousillion',
			:country_id => france.id
			)
		hugues_beauvignac = Producer.create(:name => 'Hugues Beauvignac', :established => 1985, :appellation_id => languedoc.id)
		omarine = Producer.create(:name => 'Omarine', :established => 1973, :appellation_id => languedoc.id)

		updated_france = Country.find_by(:name => 'France')

		expect(updated_france.producers).to include(hugues_beauvignac)
		expect(updated_france.producers).to include(omarine)
	end

end
```

Pretty staightforward, really.  I can't believe I was so intimidated by testing for so long!

Here's the appellation_spec file that describes the belongs_to relationship with country:

```
describe Appellation do

	it 'has a name, a tier, a region and a country_id' do 
		france = Country.create(:name => 'France')
		expect { Appellation.create(:name => 'Cotes du Rhone', :tier => 'AOC', :region => 'Rhone', :country_id => france.id) }.to_not raise_error
		languedoc = Appellation.create(
			:name => 'Languedoc',
			:tier => 'AOC',
			:region => 'Languedoc-Roussillion',
			:country_id => france.id
			)
		expect(languedoc.name).to eq('Languedoc')
		expect(languedoc.tier).to eq('AOC')
		expect(languedoc.region).to eq('Languedoc-Roussillion')
		expect(languedoc.country_id).to eq(france.id)
	end

	it 'belongs to a country' do
		france = Country.create(:name => 'France')
		languedoc = Appellation.create(
			:name => 'Languedoc',
			:tier => 'AOC',
			:region => 'Languedoc-Roussillion',
			:country_id => france.id
			)

		updated_france = Country.find_by(:name => 'France')
		updated_languedoc = Appellation.find_by(:name => 'Languedoc')

		expect(updated_france.appellations).to include(updated_languedoc)
		expect(updated_languedoc.country).to eq(updated_france)
	end
end
```

And finally, the producer_spec file that describes belongs_to country, through appellation:

```
describe Producer do 
	
	it 'has a name and an established date' do
		expect { Producer.create(:name => 'Hugues Beauvignac', :established => 1985)}.to_not raise_error
		pro = Producer.create(:name => 'Hugues Beauvignac', :established => 1985)
		expect(pro.name).to eq('Hugues Beauvignac')
		expect(pro.established).to eq(1985)
	end

	it 'belongs to an appellation' do
		france = Country.create(:name => 'France')
		languedoc = Appellation.create(
			:name => 'Languedoc',
			:tier => 'AOC',
			:region => 'Languedoc-Rousillion',
			:country_id => france.id
			)
		pro = Producer.create(:name => 'Hugues Beauvignac', :established => 1985, :appellation_id => languedoc.id)

		updated_languedoc = Appellation.find_by(:name => 'Languedoc')

		expect(pro.appellation).to eq(languedoc)
		expect(updated_languedoc.producers).to include(pro)
	end

	it 'belongs to a country through an appellation' do
		france = Country.create(:name => 'France')
		languedoc = Appellation.create(
			:name => 'Languedoc',
			:tier => 'AOC',
			:region => 'Languedoc-Rousillion',
			:country_id => france.id
			)
		pro = Producer.create(:name => 'Hugues Beauvignac', :established => 1985, :appellation_id => languedoc.id)

		updated_france = Country.find_by(:name => 'France')

		expect(pro.country).to eq(updated_france)
		expect(updated_france.producers).to include(pro)
	end

end
```
 
 
 
 

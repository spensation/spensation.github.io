---
layout: post
title:      "RSpec testing ActiveRecord has_many  :through"
date:       2018-08-27 03:27:04 +0000
permalink:  rspec_testing_activerecord_has_many_through
---


Automating my testing suite with FactoryBot and Faker hit a road block this week when I tried  to verify a has_many :through association.  I scoured the internet trying to find a good post on the subject.  While I eventually got it all wired up, it took a while and I read a few confusing posts on the matter.  So, here we go, a deep dive into testing for has_many :through!

 I'm still working on my wine cellar app and the models that I am working with are:
 
 *wine-cellar/app/models/country.rb*
```
class Country < ApplicationRecord
	has_many :appellations
	has_many :producers, through: :appellations

	validates :name, :presence => true
end

```

*wine-cellar/app/models/appellation.rb*
```
class Appellation < ApplicationRecord
	has_many :producers
	belongs_to :country

	validates :name, :region, :tier, :presence => true
end
```

*wine-cellar/app/models/producer.rb*
```
class Producer < ApplicationRecord
	belongs_to :appellation

	validates :name, :established, :presence => true
end
```

In order to use FactoryBot, we have to install the gem:

*Gemfile*
```
...
group :development, :test do
   gem 'factory_bot_rails'
end
```

run bundle install

Configure your test suite for RSpec/Rails: (otherwise check out this [link ](https://github.com/thoughtbot/factory_bot/blob/master/GETTING_STARTED.md)for other config options.)

*spec/support/factorybot.rb*

```
RSpec.configure do |config|
	config.include FactoryBot::Syntax::Methods
end

```



We also need to set up Faker, which we will use to populate our tests.  Because we are that lazy.

Back to the Gemfile!

*Gemfile*
```
...
group :development, :test do
   gem 'factory_bot_rails'
	 gem 'faker'
end
```

run bundle install

Now we have access to the Faker library.  More on that [here](https://github.com/stympy/faker).


In the Spec directory, create a new directory called factories.  In this factories directory, create a new file called countries.rb.  This will be your country factory.  More on this in a second...

The other two factories, appellation and producer, will live in a a seperate file.  Perhaps because they are the models that belong_to a country, these factories cannot live inside their respective files in spec/factories directory.  If you build them there and try to use them, you will recieve an error.  Instead, create a file in the spec directory called factories.rb.

*wine-cellar/spec/factories.rb*
```
FactoryBot.define do

	factory :appellation do 
		name { Faker::Lorem.word }
		tier { Faker::Lorem.word }
		region { Faker::Lorem.word }
		country
	
	end

	factory :producer do
		name { Faker::Name.name }
		established { Faker::Number.between(1789, 1985) }
		appellation
	end
end
```

Notice that we can pass the factories the instance of another, associated factory.  Here, by passing country to the appellation factory, we can populate the appellation table with a country.  The same is true for the producer factory and it's appellation.  Pretty cool!

Ok, back to that country factory...

This is where things get tricky.  There are a couple of things to parse in this next block of code.  

*wine-cellar/spec/factories/countries.rb*
```

FactoryBot.define do
	 factory :country do 
		  name { Faker::Address.country }

		  trait :country_with_producers do
			   transient do
				    producers_count 5  
			   end

			   after :create do |country, evaluator|
				     FactoryBot.create_list :appellation, evaluator.producers_count, country: country
			   end
		   end
	 end
end

```

So, this factory create a country and gives it a name.  That much is clear.  Then, we define a new trait for country, :country_with_producers.  This will allow us to not only test a country but also test a country in the contest of this new trait,  with producers.  We will get to the actual tests in a minute.  Now we are still in the factory, generating the objects to test.




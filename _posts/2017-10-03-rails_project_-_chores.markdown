---
layout: post
title:      "Rails Project - Chores!"
date:       2017-10-03 19:03:50 -0400
permalink:  rails_project_-_chores
---


For my Rails project, I chose to expand on the domain that I started in my Sinatra project.  My goal is to model my real life experience of living in a group house and sharing the household responsiblilties my roomates ina productive and communicative way.  Every two weeks a new chore cycle began and if your chore wasn't done by Thursday at 10pm, the public shaming began.  We used to rely on a chalkboard to hold each other accountable but I would like to give future generations of co-habitors the gift of a well thought out app.

My app has four models (to date) Users, Chores, Tasks and Cycles.  Chores have many Tasks and Tasks belong to chores.  Cycles act as the join table between Users and Chores.  Cycles also hold a key element to the success of the app, the completed boolean.  Users will be able to view the chores and tasks associated as well as the cycle that they are responsible for presently.  When they complete the chore, they can update the app and everyone will be aware that they are a rockstar roommate.  

The first thing that I had to re-master was the process of starting a new repo on github.  Well, more specifically, a new rails project.  I has used stumbled through the same process when I did my Sinatra project but I wanted to get the hang of things and I was also intimidated by the amount of stuff that was gonna get pushed up to git with a simple line like, "rails new rails-chore".

Anyway, below is the breakdown of the process. 

cd into the appropriate drive in your local environment..
run 'rails new {app-name}'
run 'git init'
run 'git add .'

Login to github and navigate to your repositories.
create a new repository with the same name as your new rails project.

Copy the remote found below this line
…or push an existing repository from the command line 
(ex. git remote add origin git@github.com:spensation/blah-blah-blah.git)

back in your terminal 
you have to set your remote to your origin.

run 'git remote add origin git@github.com:spensation/blah-blah-blah.git'
run 'git push -u origin master'

now you are ready to start coding!





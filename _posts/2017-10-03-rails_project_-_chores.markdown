---
layout: post
title:      "Rails Project - Chores!"
date:       2017-10-03 19:03:50 -0400
permalink:  rails_project_-_chores
---


For my Rails project, I chose to expand on the domain that I started in my Sinatra project.  My goal is to model my real life experience of living in a group house and sharing the household responsiblilties my roomates ina productive and communicative way.  Every two weeks a new chore cycle began and if your chore wasn't done by Thursday at 10pm, the public shaming began.  We used to rely on a chalkboard to hold each other accountable but I would like to give future generations of co-habitors the gift of a well thought out app.

My app has four models (to date) Users, Chores, Tasks and Cycles.  Chores have many Tasks and Tasks belong to chores.  Cycles act as the join table between Users and Chores.  Cycles also hold a key element to the success of the app, the completed boolean.  Users will be able to view the chores and tasks associated as well as the cycle that they are responsible for presently.  When they complete the chore, they can update the app and everyone will be aware that they are a rockstar roommate.  

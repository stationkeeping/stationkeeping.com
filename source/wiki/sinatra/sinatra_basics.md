---
title: "Sinatra Basics"
---

To use Sinatra as a Rack App, allowing POW to run it, add a ```config.ru``` file specifying the location of the app.rb and running Sinatra:

```
require 'rubygems'
require './app'

run Sinatra::Application
```
To get Sinatra app (or indeed any Rack app) to restart with every request *if you're using POW*, add a restart.txt file:

```$touch tmp/restart.txt```
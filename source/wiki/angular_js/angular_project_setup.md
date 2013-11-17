---
title: "Angular Project Setup"
---

Angular Project Setup
======================

### Client

1. Make sure you have [node](http://nodejs.org)
2. Install Yeoman, Grunt, Bower and Karma: `sudo npm install -g yo grunt-cli bower karma`
3. Install the Yeoman Angular generators: `sudo npm install -g generator-angular`
4. Create the dir for the project: `mkdir myapp && cd $_`
5. Run the Yeoman Angular generator: `yo angular`
6. To connect to the rails api using a proxy: `npm install --save-dev grunt-connect-proxy`
7. Configure Gruntfile like [this](https://gist.github.com/newtriks/7010253)

### Server

1. Set an application configuration variable in the *config.yml*:

        common: &common
           client:
             origin: http://localhost

2. To enable CORS for a public api within our `Api::BaseController` which extends `ActionController::Base` add the following:

        before_filter :set_cors_headers
        before_filter :cors_preflight
        respond_to :json

        def set_cors_headers
          headers['Access-Control-Allow-Origin'] = AppConfig.config[:client]['origin']
          headers['Access-Control-Allow-Methods'] = 'GET'
          headers['Access-Control-Allow-Headers'] = '*'
          headers['Access-Control-Max-Age'] = "3628800"
        end

        def cors_preflight
          head(:ok) if request.method == :options
        end

Existing project set up
=======================

1. Clone the repos
2. Install dependencies: `npm install && bower install`

Running the app
===============

1. **API** Terminal window one in app dir: `rails s`
2. **Client** Terminal window two in app dir: `grunt server`

General logic
=============

### Generating

To generate classes simply use Yeoman's angular [generators](https://github.com/yeoman/generator-angular#generators) e.g. `yo angular:directive placeholder` generates:

      create app/scripts/directives/placeholder.js
      create test/spec/directives/placeholder.js

### Modules

Individual file has it's own *module* and are named with their type then name  e.g. `directives.player`.

* Register with the app itself by adding it to the module dependencies array within *app.js*: `angular.module('playerApp', ['directives.player']);`
* Within the class itself ensure a new module is created by including the array syntax and include any dependencies *if* they are not already included on a global level with the app (in app.js as detailed above): `angular.module('directives.player', [])`.

### Testing

Example [karma.conf.js](https://gist.github.com/stationkeeping/7010704).

For standalone running of the test suite i.e. if there is no server running, use `grunt test:unit` or for all e2e tests use `grunt test:e2e`.

To run the test suite alongside a running server and have Karma autowatch file changes then re-run all tests use `karma start` in another terminal window.

### Grunt

**Tasks**

- `grunt server` runs the app on localhost and opens in browser
- `grunt test:unit` runs the unit tests
- `grunt test:e2e` runs the end to end tests
- `grunt stylus` compiles the stylus files to css
- `grunt build` generates a build directory in the project and compiles down the project i.e. minifying etc into a release build
- `grunt jshint` runs jshint on all js files outputting any errors to console

**Integrating [Stylus](http://learnboost.github.io/stylus/)**

1. Install Stylus: `npm install stylus`
2. Install the [Stylus Grunt plugin](https://github.com/gruntjs/grunt-contrib-stylus): `npm install grunt-contrib-stylus --save-dev`
3. Update the Gruntfile.js as detailed in this [gist revision](https://gist.github.com/newtriks/7010253/revisions)
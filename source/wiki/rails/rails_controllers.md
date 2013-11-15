---
title: "Rails Controllers"
---

Much of `ActionController` is implemented in Rack, as a series of filters. Other gems like *Warden* and *BetterErrors* are also written as Rack filters. Sometimes these filters are a way of tying gem functionality into Rack rather than the functionality existing entirely within Rack.

Show Rack Filters used by an application:

```
$ rake middleware
```

#### Request Flow 

##### Dispatching

The `routes.rb` file generates a `ActionDispatch::Routing::RouteSet` via the call to `Example::Application.routes.draw do`. The RouteSet is used to find a matching route and determine its *Rack Endpoint*. If the endpoint is a controller action, the associated dispatcher then invokes this action. All Controller actions are *Rack Endpoints*, but the endpoint could also be another Rack app, a redirect macro, or a lambda.

To manually invoke a dispatcher:

```
>> env = {}
>> env['REQUEST_METHOD'] = 'GET'
>> env['PATH_INFO'] = '/example/index'
>> env['rack.input'] = StringIO.new
ActionController::Dispatcher.new.call(env).last.body # For the response body
```

A controller's implementation of an action allows for custom handling, however, if no implementation for the action exists or the implementation is empty, it is assumed a view exists, so Rails looks for the view to render. If an action is missing, empty or no rendering is specified, Rails will do the equivalent of:

```
render "example/index"
```

##### Rendering

Rendering is the last piece of the request flow.

A controller action can render the template for a different action (for example an `update` action can render an `edit` action template if a form fails to validate):

```
def update
 render action: :update
end
```

This will render the `update` template, but will not call the `update` action, so whatever the template needs must be made available to it.

To render an explicit template:

```
render template: '/example/update.html.erb'
```

To render JSON (assuming whatever is passed has a `to_json` method):

```
render json: @example
```

or with a JSONP callback:

```
render json: @example, callback: "example_callback"
```

To render nothing:

```
render :nothing => true, :status => 401
```

###### Other options

Mime type: `:content—type`
Layout: `:layout`
Status: `:status`

##### Redirecting

Redirecting breaks the flow of the current request flow and initiates another one.

Redirect using controller action:

```
redirect_to action: show, id: 4
```

Or using a model (in which case `url_for` is called on it)

```
redirect_to @example
```

Or a URL (hardcoded or in the form of a URL helper).

Or `:back` which will return to the page that issued the request (the request's `HTTP_REFERER`) :

```
redirect_to :back
```

A flash message can also be passed (using `status:`, `alert:` or a hash value for `flash:`:

```
redirect_to example_url(@example), alert: "Nope"
```

An action will continue executing after a redirect, and will end up rendering a view, causing a `DoubleRenderError` 

#### Filters

Filters are declared at the top of a controller, and are run in the order they are declared. They can be declared together:

```
before_filter :foo, :bar
```

Or separately:

```
before_filter :foo
before_filter :bar

```

A filter is usually a method, but could also be a class which is scoped to the controller with full access to its methods:

```
class ExampleFilter
  def self.after(controller)
    # Do something
  end
end

class ExampleController < ActionController::Base
  after_filter ExampleFilter
end
```

A filter can also take the form of a block, executed in the context of the controller:

```
before_filter do
   # Do something
end
```

Filters can also be prepended using: `prepend_before_filter` and `prepend_after_filter`, or wrapped around an action using `around_filter`.

To skip a filter defined on a superclass:

```
skip_before_filter :example_filter
```

Filters can be made action-specific using:

```
before_filter :example_filter, :only => [:edit, :delete]
```

Calling `render` or `redirect_to` from a filter will halt execution of all other filters.

##### Send Data

Textual or binary data can be sent to the client in a buffer and set either to be downloaded or to be displayed inline using `send_data`.

##### Send File

`send_file` is similar to `send_data`, but rather than using data, it references a pre-existing file and hands off the writing of the file to the server, rather than Rails taking care of it itself. 



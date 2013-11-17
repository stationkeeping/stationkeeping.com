---
title: "Rails Custom Error Messages"
---

Tell rails that you want to handle errors with Routes:

```
config.exceptions_app = self.routes
```

To actually see the errors in development:

```
config.consider_all_requests_local = false
```

Delete static error pages from `public`.

Add to routes (above any catch-all matchers like High Voltage's `page#show`):

```
# Exceptions
match '/404' => 'exceptions#error_404'
match '/422' => 'exceptions#error_422'
match '/500' => 'exceptions#error_500'
```

Create the exceptions controller:

```
class ExceptionsController < ApplicationController

  layout "application"

  respond_with :html

  def error_404
    render status: :not_found
  end

  def error_422
    render status: :unprocessable_entity
  end

  def error_500
    render status: :internal_server_error
  end

end
```

Create views for each exception.

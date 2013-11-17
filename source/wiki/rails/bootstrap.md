---
title: "Bootstrap"
---

Use the `bootstrap-sass` gem for Sassy Bootstrap. 

In `assets/stylesheets/application.css.scss`:
```
@import "bootstrap";
```

In `assets/javascripts/application.js`:
```
//= require bootstrap
```

For Simple Forms integration, generate Simple Forms using:
```
rails generate simple_form:install --bootstrap
```

Make sure Devise is using your custom views (See Devise page)
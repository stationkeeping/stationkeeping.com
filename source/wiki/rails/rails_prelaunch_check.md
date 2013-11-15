---
title: "Rails Prelaunch Check"
---

#### Sensitive Parameters

Check that any sensitive params are filtered out of the logs. By default in `config/application.rb`:

```
config.filter_parameters += [:password]
```

Add any others e.g.

```
config.filter_parameters += [:password_confirmation]
```

#### Database Indexes

Check that all foreign key database columns have an index. Note that sometimes these are added implicitly.


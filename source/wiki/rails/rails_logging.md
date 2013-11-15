---
title: "Rails Logging"
---

Access the logger anywhere using `Rails.logger`.

To create a logger:

```
require 'logger'
logger = Logger.new STDOUT
```

Logs are saved to `logs` with a separate file per environment. To clear all logs:

```
$ rake log:clear
```

To tail the development log:

```
$ tail -f log/development.log
```

Or using the *Powder* gem:

```
$ powder applog
```
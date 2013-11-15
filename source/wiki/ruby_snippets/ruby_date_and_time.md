---
title: "Ruby Date And Time"
---

#### Take a commonly-formatted date - 29 March 20012 - and convert it to a DateTime (for storing in a DB)

```
date = "29 March 2013"
date_components = date.split(' ').reverse
DateTime.new(Integer(date_components[0]), Date::MONTHNAMES.index(date_components[1]), Integer(date_components[2]))
=> Fri, 29 Mar 2013 00:00:00 +0000
```

#### Take a DateTime and display as "Sat Feb 3rd, 2013"

```
date_time = DateTime.now()
date_time.strftime("%a %B #{d.day.ordinalize}, %Y")
=> "Tue March 3rd, 2013"
```
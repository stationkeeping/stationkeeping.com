---
title: "POW And XIP Basics "
---

## [POW](http://pow.cx/) [[Docs](http://pow.cx/manual.html)]

Pow lets you symlink a .dev domain to a rack application running locally. Where you previously may have been using _http://localhost:8888_, you can instead use _http://somewebapp.dev_

Setup depends on your requirements:

### Basic
If running on 3000 (Rails default), create a .dev domain by adding a symlink to the folder containing the app in the ~.pow directory.

```
$ cd ~/.pow
$ ln -s /path/to/somewebapp
```

### [Port Proxying](http://pow.cx/manual.html#section_2.1.4). 
If running on another port, create a .dev domain (via Port Proxying) by creating a file in the ~.pow directory containing the port number) with the name of your app. This means you must have app per port.

```
$ echo 8080 > ~/.pow/somewebapp
```

### Non-Rack applications
To use .pow on a non-rack application simply put site content in a public directory.

### Pow Logs

Pow stores a log for each app in ```~/Library/Logs/Pow/apps/```. To Tail the log:

``` 
$tail -f ~/Library/Logs/Pow/apps/my-app-name.log
```

## [XIP.IO](http://xip.io/)

XIP.IO allows you to access your dev machine from other devices on the same LAN using your machine's IP Address.

Example (Assuming a rack app is running as somesite.dev on a machine with an IP of 192.168.1.4:

```
http://somesite.192.168.1.4.xip.io
```

**Note that there is no .dev before the IP Address and that the IP is of the the machine running Rack app, not the one connecting to it**

If this stops working suddenly, check that the IP address of the machine hasn't changed using System Prefs/Network to get its IP address.

This will allow Typekit fonts to work without restriction.
It also works nicely with [Adobe Edge Inspect](http://html.adobe.com/edge/inspect/) for testing devices.
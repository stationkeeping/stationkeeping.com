---
title: "Sublime Text Packages"
---

Useful packages and how to use them.

# UI

### Side Bar Enhancements
[Site](https://github.com/titoBouzout/SideBarEnhancements/tree/st3)

Improved right-click options in the sidebar

### Sublime Terminal
[Site](http://wbond.net/sublime_packages/terminal

Open Terminal at Files or Project. Easily configure it to use iTerm2.

# Syntax

## General

### Alignment
[Site](http://wbond.net/sublime_packages/alignment)
*Ctrl + Cmd + A*

Alignment of multi-line selections and multiple selections to a common character:

In JavaScript:
```
var one = 1;
var three = 3;
var fifteen = 15;
```
Becomes:
```
var one     = 1;
var three   = 3;
var fifteen = 15;
```

In Sass:
```
padding: 8px 24px 8px 6px;
border-top: 1px solid black;
```
Becomes:
```
padding     : 8px 24px 8px 6px;
border-top  : 1px solid black;
```

### Change Quotes
[Site](https://github.com/colinta/SublimeChangeQuotes)

Change single to double or double to single quotes.
Note: There is no key binding - it is used from the command pallette: `ChangeQuotes`


### Sublime Linter
[Site](https://github.com/SublimeLinter/SublimeLinter)

Currently to install the ST3 Version:
```
$ cd ~/.config/sublime-text-3/Packages
$ git clone https://github.com/SublimeLinter/SublimeLinter.git
$ cd SublimeLinter
$ git checkout sublime-text-3
```


## JavaScript

### JSFormat
[Site](https://github.com/jdc0589/JsFormat}

Formats JavaScript files using JSBeautifier. 

### BeautifyRuby
[Site]https://github.com/CraigWilliams/BeautifyRuby
*Ctrl + Cmd + K*

Format Ruby code (including in .erb files) using the Ruby Script Beautifier.
Note: You will probably need to set the ruby path in its  User settings. 

### Ruby Test
[Site](https://github.com/maltize/sublime-text-2-ruby-tests)

Ruby Test allows you to run tests from sublime text. Currently need to launch ST from the command line so that it has access to envs defined in `.bash_profile`.
---
title: "Sass Structure"
---

```
main.css.scss  # Entry Point
   - vendor/html5boilerplate/defaults # Boilerplate uses Normalizr, but replace with desired reset
   - base.css.scss
       - utils # Sass functions
       - colours 
       - fonts 
       - elements # Generic HTML elements
       - sprites  
       - grid # Site-wide grid setup - nothing page specific, but generic modules are OK
```

Examples:

```
base/_colours.cs.scss

# Set colours
$red         : #F83739;
$blue        : #6ED9D9;
$yellow      : #EFEF1D;
$green       : #21FFBC;
$black       : #050505;
$white       : #F7F5E2;
$grayDark    : #363738;

# Assign colours to semantic classes that are not named after actual colour so changing a colour only effects this file.

$filmColour  : $red;
$musicColour : $blue;
$newsColour  : $yellow;
$aboutColour : $green;
```

```
base/_fonts.css.scss

# Assign Fonts
$brandonGrotesque : "brandon-grotesque", "helvetica neue", helvetica, Arial, sans-serif;
$centuryGothic    : "Century Gothic", CenturyGothic, AppleGothic, sans-serif;

# Assign fonts to semantic classes that are not named after the actual font so that changing a font only effects this file.
$titleFontFamily  : $brandonGrotesque;
$bodyFontFamily   : $centuryGothic;
```
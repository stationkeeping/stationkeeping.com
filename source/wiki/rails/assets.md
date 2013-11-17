Use Sass for Styles.

Set up file structure as follows:

`assets/stylesheets/application.css.scss`:

```
/*
 *= require_self
 */

@import 'compass'; // Compass mixins
@import 'bootstrap'; // Includes reset
@import 'base'; // Base styles
@import 'modules' // Page modules
```

`assets/stylesheets/base.css.scss`:
```
@import 'base/colours';
@import 'base/fonts';
@import 'base/icons';
@import 'base/elements';
```

#### Fonts

Set up a Typekit kit with project fonts. Add local IP addresses (e.g. 192.168.1.2), POW .dev domains, Heroku subdomains for production and staging and final domains.

Add fonts embed code to the `<head>` of the HTML/ERB template:

```
<script type="text/javascript" src="//use.typekit.net/XXXXX.js"></script>
<script type="text/javascript">try{Typekit.load();}catch(e){}</script>
```
---
title: "Middleman Basics"
---

## Setup

`$ gem install middleman`

Navigate to directory that will contain your project directory:

`$ middleman init project_name --template=html5`

Add elements to .gitignore.

Add gems to Gemfile:

```
# Sync Assets to S3 (wraps Asset Sync)
gem 'middleman-sync'

# Add support for Sprockets including Fingerprinting
gem 'middleman-sprockets'

# Automatically reload the page on changes to the source
gem 'middleman-livereload'

# Minify HTML Output
gem 'middleman-minify-html'

# Add support for responsive grids
gem 'susy'

# Load environmental variables from .env file
gem 'dotenv'

# Generate touch and favicon icons using a single base image
gem 'middleman-favicon-maker'

# Markdown
gem 'redcarpet'
```

Install gems:

`$ bundle install`

Add '.env' file to root for use with the dot-env gem:

```
AWS_ACCESS_KEY_ID=XXXXXXXXXXX
AWS_SECRET_ACCESS_KEY=XXXXXXXXXX
AWS_REGION=eu-west-1
AWS_BUCKET=XXXXXXXXXXXXXXX
```


Add/Uncomment gem setup in `config.rb`:

```
# Susy grids in Compass
require 'susy'

# Markdown Engine
set :markdown_engine, :redcarpet

require 'middleman-sprockets'

# Load Envs
Dotenv.load

# Change Compass configuration
compass_config do |config|
  config.output_style = :compact
end
```

Add/uncomment build-specific Gem activation in `config.rb`:

```

# Create gzipped versions of files
activate :gzip

# Build-specific configuration
configure :build do
  # For example, change the Compass output style for deployment
  activate :minify_css

  # Minify Javascript on build
  activate :minify_javascript

  # Genrate Touch icons and Favicon
  activate :favicon_maker

  # Sprockets
  activate :sprockets;

  # Live Reload
  activate :livereload

end
```

Add helpers to `config.rb`

```
# Methods defined in the helpers block are available in templates
helpers do

  # Page specific ID applied to body tag
  def page_id
    data.page.page_name.downcase().tr(' ', '_')
  end
  
  # Page title
  def page_title
    'Page Title Base | '+ data.page.page_name
  end
  
  # Range of Copyright
  def copyright_years(start_year)
    end_year = Date.today.year
    if start_year == end_year
      start_year.to_s
    else
      start_year.to_s + '-' + end_year.to_s
    end
  end

  # Site Copyright
  def site_copyright(start_year)
    dates = copyright_years(start_year)
    "&copy; #{dates} XXXXXXXX SITE OWNER XXXXXXXX"
  end

end
```

Add head-loading JavaScript libs to `source/layouts/layout.erb`:

Fonts:

Typekit:

```
<!-- Make Typekit Kit available -->
<script type="text/javascript" src="//use.typekit.net/XXXXX.js"></script>
<script type="text/javascript">try{Typekit.load();}catch(e){}</script>
```

Fonts.com:
```
<!-- Make Fonts.com project fonts available -->
<script type="text/javascript" src="http://fast.fonts.com/jsapi/XXXXX.js"></script>
```

Add post-body loading JavaScript Libs to `source/layouts/layout.erb`:

```
<!-- Added JavaScript Libs-->

<!-- Media query support for IE8 -->
<script src="js/vendor/respond.js"></script>

<!-- Dynamically generated placeholder images -->
<script src="js/vendor/holder.js"></script>

<!-- Indeterminate Loader -->
<script src="js/vendor/spin.js"></script>

<!-- Templating -->
<script src="js/vendor/handlebars.js"></script>

<!-- Animation library -->
<script src="http://cdnjs.cloudflare.com/ajax/libs/gsap/1.9.0/TweenMax.min.js"></script>
```

Add js files for these libs to `source/js/vendor`

Add Deprecated Browser Alert to `source/layouts/layout.erb`:

```
<!-- Deprecated Browser Alert -->
<!--[if lt IE 8]>
   <div class="deprecated-browser-warning">
      <%= image_tag('ie_logo.png') %>
      <h3>You are using an outdated browser which is no longer supported by this site.</h3>
      <p><a href="http://browsehappy.com/">Upgrade your browser for free today</a> or <a href="http://www.google.com/chromeframe/?redirect=true">install Google Chrome Frame</a> to experience this site.</p>
   </div>
<![endif]-->
```

Add page specific ID to body tag of `source/layouts/layout.erb`

`<body class="<%= page_id %>">`

Add page-specific JavaScript loading to post-body:

```
<!-- Page Specific Javascripts -->
<% if content_for?(:page_specific_javascripts) %>  
   <%= yield_content :page_specific_javascripts %>
<% end %>
```

Add template-specific JavaScript to post-body:

```
<!-- Page Specific Templates -->
<% if content_for?(:page_specific_templates) %>  
   <%= yield_content :page_specific_templates %>
<% end %>
```

Change Page Title to use helper:

```
<title><%= page_title %></title>
```

Move normalize.css to `source/css/vendor`
Delete Boilerplate LICENCE and README files.
Delete Boilerplate favicon and touch icons.

Change frontmatter entry in `source/index.html.erb` from 'title' to 'page_name'

Change main css entry point in source/layouts/layout.erb to `source/css/site/main.css.scss`

Edit `source/css/site/main.css.scss`:

```
//= require vendor/normalize.css

// Boilerplate Defaults
@import "vendor/html5boilerplate/defaults";

// Site Base
@import "site/base";
@import "site/modules";
@import "site/layout";
 
// Boilerplate Print Overrides
@import "vendor/html5boilerplate/print";
```

Add sass files (See separate page)

Fill out meta description:

```
<meta name="description" content="XXXXXXXXXX">
```
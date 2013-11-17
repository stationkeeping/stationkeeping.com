---
title: "Paperclip"
---

#### System Dependencies

Install Image Magick
```
brew install imagemagick
```

If you want PDF support install GhostScript:
```
brew install gs
```

#### Rails Project

Tell Paperclip to use S3 in `config/initializers/paperclip.rb`:

```
Paperclip::Attachment.default_options[:url] = ':s3_domain_url'
```

Then tell Paperclip where to find ImageMagick `config/development.rb`:
```
Paperclip.options[:command_path] = "/usr/local/bin/identify"
```

Add Paperclip to a Model:
```
class Example < ActiveRecord::Base
  attr_accessible :attachment
  has_attached_file :attachment
end
```

Add a migration:
```
class AddAttachmentToExample < ActiveRecord::Migration
  def change
    add_attachment :examples, :attachment
  end
end
```

This will generate 

Paperclip adds the following properties to your model:
```
attachment_file_name
attachment_file_size
attachment_content_type
attachment_updated_at
```

Set defaults in `config/application.rb`:
```
  config.paperclip_defaults = {
  :storage => :s3,
  :s3_credentials => {
    :bucket => ENV['AWS_BUCKET'],
    :access_key_id => ENV['AWS_ACCESS_KEY_ID'],
    :secret_access_key => ENV['AWS_SECRET_ACCESS_KEY'],
    :endpoint => ENV['AWS_REGION']
    }
  }
```

#### Validations
```
validates_attachment_presence
validates_attachment_content_type
validates_attachment_size
```

Or at the same time:

```
validates_attachment :avatar, :presence => true,
  :content_type => { :content_type => "image/jpg" },
  :size => { :in => 0..10.kilobytes }
```

#### Image Processing

Specify image processing in `has_attached_file` on the model

```
has_attached_file :attachment, …
```

Values are as follows:

`s3_host_name` is the S3 host used to store the files:
```
:'s3-eu-west-1.amazonaws.com'
```

`url` specifies the location to save the images to if remote, using symbols to interpolate values:
```
url: "uploads/:basename_:style.:extension",
```

`path` specifies the location to save the images to if local, using symbols to interpolate values:
```
path: "uploads/:basename_:style.:extension",
```


   ,
   styles: { :medium => "300x300>"}

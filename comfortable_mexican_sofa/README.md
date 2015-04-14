# Comfortable Mexican Sofa

### Official Wiki: https://github.com/comfy/comfortable-mexican-sofa/wiki

## Testing for version: 
```ruby
gem 'comfortable_mexican_sofa' 1.12.8
```

Details: https://github.com/comfy/comfortable-mexican-sofa/releases

##Install dependencies:

##### auto:

#### active_link_to 1.0.2
#### autoprefixer-rails 5.1.8.1
#### bootstrap-sass 3.3.4.1
#### bootstrap_form 2.3.0
#### climate_control 0.0.3
#### cocaine 0.5.7
#### codemirror-rails 5.0
#### haml 4.0.6
#### sexp_processor 4.5.0
#### ruby_parser 3.6.5
#### html2haml 2.0.0
#### haml-rails 0.9.0
#### kramdown 1.6.0
#### paperclip 4.2.1
#### plupload-rails 1.2.1
#### comfortable_mexican_sofa 1.12.8

##### manual:

#### gem paperclip , "~> 4.2" (stable)

#### will_paginate, '~> 3.0.6' (stable, works with Sinatra, Rails & Merb)
### OR
#### kaminari

#### 0. Paperclip's setup (see bottom of file)

More details on Paperclip: https://github.com/thoughtbot/paperclip

####1. Basic will_paginate usage (see bottom of file)

## Setting Up

After adding the gem to the projects Gemfile, run these on the terminal while in the app's directory:
```bash
bundle install
rails generate comfy:cms
rake db:migrate
```

#### See generated migrations at db/migrate in detail to have an idea of how is Comfy's overall modeling.

These will generate an initializer file (inside config/initializers), migration files, CMS fixtures and routing sets.

Take a look at 'routes.rb':
```ruby
comfy_route :cms_admin, :path => '/admin'
comfy_route :cms, :path => '/', :sitemap => false
```

These should have been generated in this order. If that's the case, everything is fine. If it's not, add these routes in this specific order and check if the app is behaving as it should.

After that go to http://app_route/admin to start populating content.
Default username and password are: username // password

Default username and password can be configured in 'config/initializers/comfortable_mexican_sofa.rb':
```ruby
ComfortableMexicanSofa::HttpAuth.username = 'username' # Initially at line 99
ComfortableMexicanSofa::HttpAuth.password = 'password' # Initially at line 100
```

Since these are freely displayed, it is important to add the gem's configuration file to the .gitignore file before sending the app to production mode with the final username and password.

## Main Generators (extracted from official documentation, to be used after content is created inside cms):
```ruby
rails g comfy:cms:views # copy all cms view templates to your application. Now you can change if you want wysiwyg or normal text for snippets yourself. It's back to plain text by default, by the way.
rails g comfy:cms:controllers # copy all cms controllers to your application.
rails g comfy:cms:models # copy all cms models to your application.
rails g comfy:cms:assets # copy all cms js, css and image files to your application.
```
Running these generators will not only enable customization of both content rendered and admin pannel, but will also allow for customizing validations in model and making a local backup of the website's content.
## Content Creation

### Layout
First of all, it is needed to create a layout. It will add some basic tags to the content and also display a box for each specific layout division when a page is created under that layout. The form for the layout is the following:

#####Layout Name: a name to be associated and displayed when searching through the created layouts in the CMS.
#####Identifier: an identifier associated with the layout, to be selected from the drop-down box that will be available when creating a new page to associate that new page with the layout.
#####App Layout: The 'de facto' overall app's layout, edited in the app project itself (not in the CMS). Displays all the files under '/app/views/layouts' as an option. The content rendered through the customized CMS layout is rendered in the <%= yield %> space.
#####Content: see 3.Layout Structure below
#####Stylesheet: Define stylesheets here; not very useful since these can be customized in a text editor.

#### Layout structure

####0. Paperclip's Setup
Install imagemagick using Homebrew:
```ruby
brew install imagemagick
```

To enable PDF uploading (or to run a test suite for files), GhostScript must be installed as well:
```ruby
brew install gs
```
To use paperclip while in development mode, add this to config/environments/development.rb :
```ruby
Paperclip.options[:command_path] = "/usr/local/bin/"
```

Example of Paperclip's usage in a model:
```ruby
class User < ActiveRecord::Base
  has_attached_file :avatar, :styles => { :medium => "300x300>", :thumb => "100x100>" }, :default_url => "/images/:style/missing.png"
  validates_attachment_content_type :avatar, :content_type => /\Aimage\/.*\Z/
end
```

Example of adding a migration for a Paperclip attribute (equivalent to run 'rails generate paperclip user avatar' in term):
```ruby
class AddAvatarColumnsToUsers < ActiveRecord::Migration
  def self.up
    add_attachment :users, :avatar
  end

  def self.down
    remove_attachment :users, :avatar
  end
end
```

More details on Paperclip: https://github.com/thoughtbot/paperclip

####1. will_paginate's usage:
```ruby
## perform a paginated query:
@posts = Post.paginate(:page => params[:page])

# or, use an explicit "per page" limit:
Post.paginate(:page => params[:page], :per_page => 30)

## render page links in the view:
<%= will_paginate @posts %>

# for the Post model
class Post
  self.per_page = 10
end

# set per_page globally
WillPaginate.per_page = 10

# paginate in Active Record now returns a Relation
Post.where(:published => true).paginate(:page => params[:page]).order('id DESC')

# the new, shorter page() method
Post.page(params[:page]).order('created_at DESC')
```

will_paginate's styling: http://mislav.uniqpath.com/will_paginate/
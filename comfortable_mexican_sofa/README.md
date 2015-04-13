# Comfortable Mexican Sofa

### Official Wiki: https://github.com/comfy/comfortable-mexican-sofa/wiki

## Testing for version: 
```ruby
gem 'comfortable_mexican_sofa' 1.12.8
```

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

####2. See generated migrations at the bottom of the file.

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

## Content Creation

### Pagination Elements

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

####2. Migration results
== 20150413152513 CreateCms: migrating ========================================
-- create_table(:comfy_cms_sites)
   -> 0.0216s
-- add_index(:comfy_cms_sites, :hostname)
   -> 0.0008s
-- add_index(:comfy_cms_sites, :is_mirrored)
   -> 0.0011s
-- create_table(:comfy_cms_layouts)
   -> 0.0012s
-- add_index(:comfy_cms_layouts, [:parent_id, :position])
   -> 0.0006s
-- add_index(:comfy_cms_layouts, [:site_id, :identifier], {:unique=>true})
   -> 0.0011s
-- create_table(:comfy_cms_pages)
   -> 0.0011s
-- add_index(:comfy_cms_pages, [:site_id, :full_path])
   -> 0.0006s
-- add_index(:comfy_cms_pages, [:parent_id, :position])
   -> 0.0011s
-- create_table(:comfy_cms_blocks)
   -> 0.0008s
-- add_index(:comfy_cms_blocks, [:identifier])
   -> 0.0006s
-- add_index(:comfy_cms_blocks, [:blockable_id, :blockable_type])
   -> 0.0011s
-- create_table(:comfy_cms_snippets)
   -> 0.0010s
-- add_index(:comfy_cms_snippets, [:site_id, :identifier], {:unique=>true})
   -> 0.0007s
-- add_index(:comfy_cms_snippets, [:site_id, :position])
   -> 0.0009s
-- create_table(:comfy_cms_files)
   -> 0.0009s
-- add_index(:comfy_cms_files, [:site_id, :label])
   -> 0.0005s
-- add_index(:comfy_cms_files, [:site_id, :file_file_name])
   -> 0.0010s
-- add_index(:comfy_cms_files, [:site_id, :position])
   -> 0.0012s
-- add_index(:comfy_cms_files, [:site_id, :block_id])
   -> 0.0011s
-- create_table(:comfy_cms_revisions, {:force=>true})
   -> 0.0005s
-- add_index(:comfy_cms_revisions, [:record_type, :record_id, :created_at], {:name=>"index_cms_revisions_on_rtype_and_rid_and_created_at"})
   -> 0.0005s
-- create_table(:comfy_cms_categories, {:force=>true})
   -> 0.0012s
-- add_index(:comfy_cms_categories, [:site_id, :categorized_type, :label], {:unique=>true, :name=>"index_cms_categories_on_site_id_and_cat_type_and_label"})
   -> 0.0010s
-- create_table(:comfy_cms_categorizations, {:force=>true})
   -> 0.0006s
-- add_index(:comfy_cms_categorizations, [:category_id, :categorized_type, :categorized_id], {:unique=>true, :name=>"index_cms_categorizations_on_cat_id_and_catd_type_and_catd_id"})
   -> 0.0006s
== 20150413152513 CreateCms: migrated (0.0455s) ===============================
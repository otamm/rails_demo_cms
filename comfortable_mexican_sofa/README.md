# Comfortable Mexican Sofa

### Official Wiki: https://github.com/comfy/comfortable-mexican-sofa/wiki

## Testing for version: 
```ruby
gem 'comfortable_mexican_sofa' 1.12.8
```

##Installed dependencies:
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

After adding the gem to the projects Gemfile, run these on the terminal while in the app's directory:
```bash
bundle install
rails generate comfy:cms
rake db:migrate
```

These will generate an initializer file (inside config/initializers), migration files, CMS fixtures and routing sets.

Take a look at 'routes.rb':
```ruby
comfy_route :cms_admin, :path => '/admin'
comfy_route :cms, :path => '/', :sitemap => false
```

These should have been generated in this order. If that's the case, everything is fine. If it's not, add these routes in this specific order and check if the app is behaving as it should.

After that go to http://app_route/admin to start populating content.
Default username and password are: username // password

####1. See generated migrations at the bottom of the file.


####1.
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
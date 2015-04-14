#Locomotive

Official documentation: http://doc.locomotivecms.com/

Dependant on ImageMagick, Ruby 1.9.3 (or higher versions) and MongoDB. Rails3.x is also listed as a dependency but my computer doesn't have it and everything in Wagon ran without problems. If it is a dependency for the Engine, maybe the local server should have it?

Gem version is automatically provided (v 1.5.4 as of today, 14/04/2015)
Locomotive is architechted in two ways, having the 'Engine' and the 'Wagon' parts: 

#### Engine

The Engine is basically a Rails engine, requiring some initial set up and managing page request, the RESTful API and also the app's parts where the content creators will add pages to the site. Usually the Engine only needs some set up for the local machine and that's it.

#### Wagon

A command-line tool that enables development for Locomotive from the local computer. Wagon enables scaffolding generator for a Locomotive site, which enables content/template editing and adding from a regular text editor. The site can be previewed by running '$ wagon serve' on the terminal. Once the site is finished, it can be deployed to the local LocomotiveCMS with the '$wagon push' command. Wagon has native support for SASS, LESS, HAML and CoffeeScript.

## Installing Wagon
Full instructions:
http://doc.locomotivecms.com/get-started/install-wagon
If you are working in an already set-up machine (with Ruby, Rails, RubyGems and Imagemagick installed), basically the only needed component will be the gem (must be installed directly in the local computer):
```ruby
gem install locomotivecms_wagon
```

## Post-Install Steps
To create a new app, run '$wagon init your_app_name' on the terminal, of course replacing 'your_app_name' by the name you want to give to your app (note that that is the command which will generate the initial project folder, NOT '$rails new your_app_name'). Near the end of the generation of files, the terminal will stop the execution to ask if you want to use HAML templates. Simply type 'yes' or 'no' to that question. After the initial generation process is finished, the terminal will output some commands to get you started:
```ruby
cd ./loco_try # go to your app's directory (even though I've changed the directory's name to "locomotive", I've initialized it with the 'loco_try' name).
bundle install # in your app's directory, run 'bundle install' to finish the installation of the app's dependencies.
bundle exec wagon serve # wagon's equivalent to 'bundle exec rails server'
open http://0.0.0.0:3333 # opens the given host at your default browser. Note that while Rails operates in localhost:3000, wagon operates in localhost:3333 (or 0.0.0.0:3333 , whatever).
```
After running the app in the local computer's server for the first time, you should be redirected to a default generated view located in 'app/views/pages/index.liquid' . Now, why does it end with '.liquid'?

####LiquidMarkup

Liquid is another code-rendering-in-html such as ERB and HAML. It is a dependency of Wagon, so it should be understood to use Locomotive. However, it is very simple. To briefly give an idea, the following two code snippets are equivalent:

######ERB:
```ruby
<% final_verdict = "sucks" %>
<h1 class="website_verdict">This website <strong><%= final_verdict %></strong>.</h1>
```
######LiquidMarkup:
```ruby
{% final_verdict = "sucks" %}
<h1 class="website_verdict">This website <strong>{{ final_verdict }}</strong>.</h1>
```
So '{{ }}' are "output code" tags and '{% %}' are "background page logic" tags. See this repo's 'app/views/pages/index.liquid' for a more complete demonstration. Liquid's official website: http://liquidmarkup.org/

## Initial Configuration & Basic Understanding of Generated Resources
The app's main configuration is under './config', of course. The configuration files are all YAML files, the main file being 'site.yml' . Each individual configuration option is well explained there, so I'll jump to how to use the configuration in the view pages.
Take the 'name:', for instance. If you go to 'app/views/pages/index.liquid', you'll notice that inside of the <title> HTML tags there's this LiquidMarkup code snippet:
```ruby
<title>{{ page.title }} | {{ site.name }}</title>
```

Both 'page' and 'site' are default variables provided by Locomotive itself (of local and global access, respectively). 'page' accesses the YAML contained on top of the page:
```ruby
---
title: Welcome to Acme
---
```
'site' on the other hand accesses the 'name' defined in 'config/site.yml', which is accessible throughout the entire app.

This feature provides both convenience for a non-technical administrator to fully edit content (both the YAML page header's and 'config/site.yml' file's attributes are editable in the app's back office GUI) and to maintaing the app totally dynamic (index.liquid will serve as a template for other pages as well).

Also, note the '{{ 'main.css' | stylesheet_tag }}'. This will point to './public/stylesheets/main.css'. The basic filter in LiquidMarkup works by being placed inside an output tag with the file name in quotes followed by a pipe character and then having the individual filter specified (stylesheet_tag in this case).

The second filter is the image's. It points to './public/images/logo.jpg' and is inside a div with a "#logo" id, being affected by whatever is specified in 'main.css' about the #logo.

After that theres the "{% block 'main' %}" which renders the specific page's content, more or less equivalent to the default '<%= yield %>' ERB statement present in the body of a regular layout in a Rails app. Note also that in Liquid the specific content is rendered by a block itself (not by a yielded external block) and instead of the regular "end" to close a block in Ruby, this block is closed by a "{% endblock %}" statement. In Liquid, "a block tag is supposed to contain code to be overwritten by a child template".

The {% editable_text %} tag indicates a text passage that can be edited from the app's back office; so, for example, instead of asking a developer to edit the fixed text inside the page's template, a non-technical website editor can edit this passage him/herself.

## Creating New Content

As of this moment, the only page in the app is the index layout page. Even though the main nav contains a link pointing to a '/products' sessions, this session is unexistent still. Let's create it by running this terminal command in the project's root directory:

```ruby
bundle exec wagon generate content_type products title:string description:text
```
This will generate the "products" content type with two fields: title (a plain string) and description(a text, which is a string of longer size). The arguments that generates this initial set-up for the 'products' content is not required, it's just a convenient way of providing a basic initial set-up in the generated resources.
The command will add two files to the project:
- "app/content_types/products.yml": basic configuration file for products' pages. See file for a detailed overview of the configuration option (all of them are customizeable from the app's back-office admin area).
- "data/products.yml": Data to be displayed in pages. The first 4 samples were auto-generated, the rest was added.

The category's template file must be manually created and its content's filled in 'app/views/pages/products.liquid' .
Take a look at this file. You'll notice a very similar structure to 'app/views/pages/index.liquid' ; however, there are some differences.

See the '{% extends parent %}' Liquid tag for example. In Locomotive, all page templates are part of an inheritance tree, the base being 'index.liquid'. This tag makes the page a copy of its parent by default, so for instance if 'products.liquid' was totally blank except for the YAML at the top and the '{% extends parent %}' tag, the page would look exactly like its parent except for any fields that might be populated with a {{ page.title }}, which would be overriden by the title defined in the child page.
The new content for the child page can be unique and not merely a copy of its parent's due to the "main" block defined inside it. The content inside of a child page's 'main' block overrides the one defined in its parent's "main" block, so the content of the "products.liquid" page will be defined inside the page's own "main" block instead of just copying the whole content of "index.liquid".

Inside of 'products.liquid' "main" block we have a 'for' loop (note that the loop is closed by a "endfor" tag instead of a regular "end") that iterates through the attribute "products" of "contents". "contents" is a default Locomotive variable with a global scope and accesses the content defined inside of the "app/content_types" directory. Each looping will access the attributes "title" and "description" of the product being currently iterated on and output each attribute's content in the space defined in the "products.liquid" file.
Also note that "title" is a default attribute name in LocomotiveCMS and will render the content of the string defined after the score "-" in "data/products.yml" (the first product will have the title of "Sample 1", for example).

Since "products.liquid" is a child of "index.liquid", the display of "products.liquid" will contain everything defined in "index.liquid", except for any YAML attributes overriden in the top of the child page (such as 'title') and the content of "index"'s "main" block.

Also note that any CSS or Javascript can be pointed from a child page and also override its parent's front-end.

## Further Documentation

#### Launching LocomotiveCMS into Production: http://doc.locomotivecms.com/get-started/engine-in-production

#### Setting-up LocomotiveCMS into a local server: http://doc.locomotivecms.com/get-started/install-engine-locally

  
#Locomotive
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
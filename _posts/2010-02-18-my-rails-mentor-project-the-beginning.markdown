---
layout: post
title: my rails mentor project - the beginning
date: 2010-02-18 18:30:00 -08:00
categories:
  -- ruby
  -- rails
  -- github
  -- mentorship
  -- rails project
---

I mentioned in my previous [post](/2010/02/17/my-learning-resources/) that I requested a mentorship with [Kris](http://www.railsmentors.org/users/237) at [Rails Mentors](http://railsmentors.org/).  I was fortunate he accepted my request and I spoke to him over the weekend via Skype to discuss about what I want to get out of this mentorship.  I told him I wanted to start a small Rails project, specifically, build a website that can create rss feeds for rss-less websites.  Kris liked the idea and created a [private](https://github.com/kris/mentor-sl4m) github repo.  He sent me his environment.rb file and my job is to look into the gems he likes to use in his Rails projects.

{% highlight ruby %}
# Be sure to restart your server when you modify this file

# Specifies gem version of Rails to use when vendor/rails is not present
RAILS_GEM_VERSION = '2.3.5' unless defined? RAILS_GEM_VERSION

# Bootstrap the Rails environment, frameworks, and default configuration
require File.join(File.dirname(__FILE__), 'boot')

Rails::Initializer.run do |config|
  # Settings in config/environments/* take precedence over those specified here.
  # Application configuration should go into files in config/initializers
  # -- all .rb files in that directory are automatically loaded.

  # Add additional load paths for your own custom dirs
  config.load_paths += %W(Rails.root.join('app', 'middleware'))

  # Specify gems that this application depends on and have them installed with rake gems:install
  config.gem 'acl9', :source => 'http://gemcutter.org'
  config.gem 'redis', :source => 'http://gemcutter.org'
  config.gem 'redis-store', :source => 'http://gemcutter.org'
  config.gem 'xapit', :source => 'http://gemcutter.org'
  config.gem 'gravtastic', :source => 'http://gemcutter.org'
  config.gem 'paperclip', :source => 'http://gemcutter.org'
  config.gem 'stringex', :source => 'http://gemcutter.org'
  config.gem 'email_veracity', :source => 'http://gemcutter.org'
  config.gem 'state_machine', :source => 'http://gemcutter.org'
  config.gem 'acts_as_tree', :source => 'http://gemcutter.org'
  config.gem 'formtastic', :source => 'http://gemcutter.org'
  config.gem 'authlogic', :source => 'http://gemcutter.org'
  config.gem 'searchlogic', :source => 'http://gemcutter.org'
  config.gem 'settingslogic', :source => 'http://gemcutter.org'
  config.gem 'hoptoad_notifier', :source => 'http://gemcutter.org'
  config.gem 'sitemap_generator', :source => 'http://gemcutter.org'
  config.gem 'validates_timeliness', :source => 'http://gemcutter.org'
  config.gem 'inherited_resources', :source => 'http://gemcutter.org'
  config.gem 'acts-as-taggable-on', :source => 'http://gemcutter.org'
  config.gem 'will_paginate', :source => 'http://gemcutter.org'
  config.gem 'bullet', :source => 'http://gemcutter.org'
  config.gem 'factory_girl', :source => 'http://gemcutter.org'
  config.gem 'rspec', :lib => false, :source => 'http://gemcutter.org'
  config.gem 'rspec-rails', :lib => false, :source => 'http://gemcutter.org'
  config.gem 'webrat', :source => 'http://gemcutter.org'
  config.gem 'cucumber', :source => 'http://gemcutter.org'

  # Only load the plugins named here, in the order given (default is alphabetical).
  # :all can be used as a placeholder for all plugins not explicitly named
  # config.plugins = [ :exception_notification, :ssl_requirement, :all ]

  # Skip frameworks you're not going to use. To use Rails without a database,
  # you must remove the Active Record framework.
  config.frameworks -= [ :active_resource ]

  # Activate observers that should always be running
  # config.active_record.observers = :cacher, :garbage_collector, :forum_observer

  # Set Time.zone default to the specified zone and make Active Record auto-convert to this zone.
  # Run "rake -D time" for a list of tasks for finding time zone names.
  config.time_zone = 'UTC'

  # The default locale is :en and all translations from config/locales/*.rb,yml are auto loaded.
  # config.i18n.load_path += Dir[Rails.root.join('my', 'locales', '*.{rb,yml}')]
  config.i18n.default_locale = :en
end
{% endhighlight %}
---
layout: post
title: rails 3 rc on windows using rubyinstaller 1.9.2
date: 2010-08-21 12:00:00 -05:00
categories:
  -- ruby
  -- rails
---

*Update:* Rails 3 is now available.  The instructions below should also work for the final release.

*Update 2:* SQL2 gem (starting with version 0.2.5) works for Windows.  See [gist](http://gist.github.com/635442).

This week the Ruby team [released](http://blade.nagaokaut.ac.jp/cgi-bin/scat.rb/ruby/ruby-talk/367983) the most anticipated Ruby version, 1.9.2, and yesterday, the RubyInstaller team [released](http://groups.google.com/group/rubyinstaller/browse_thread/thread/67902a04f12cc726#) 1.9.2 for Windows.  Kudos to both teams for the hard work and dedication.

## Requirements

* [RubyInstaller 1.9.2-p0](http://rubyforge.org/frs/download.php/72170/rubyinstaller-1.9.2-p0.exe) 
* [DevKit-4.5.0-20100819-1536-sfx.exe](http://github.com/downloads/oneclick/rubyinstaller/DevKit-4.5.0-20100819-1536-sfx.exe)
* SQLite3 or MySQL

## Installation

Installing Ruby using RubyInstaller should be straight forward.  The installer automatically defaults the path to *C:\Ruby192*.  If you have used DevKit in the past, you'll notice this DevKit requires a different way to install.  I highly recommend following the instructions [here](http://wiki.github.com/oneclick/rubyinstaller/development-kit).  You'll need to remove the old DevKit install for other Ruby installations and install the new DevKit using the *dk.rb* script.

Install Rails 3 using the *--pre* parameter.  *Update:* --pre is no longer necessary now that Rails 3 is official

{% highlight text %}
c:\devkit>gem install rails --pre
Temporarily enhancing PATH to include DevKit...
Successfully installed activesupport-3.0.0.rc
Successfully installed builder-2.1.2
Successfully installed i18n-0.4.1
Successfully installed activemodel-3.0.0.rc
Successfully installed rack-1.2.1
Successfully installed rack-test-0.5.4
Successfully installed rack-mount-0.6.11
Successfully installed tzinfo-0.3.23
Successfully installed abstract-1.0.0
Successfully installed erubis-2.6.6
Successfully installed actionpack-3.0.0.rc
Successfully installed arel-0.4.0
Successfully installed activerecord-3.0.0.rc
Successfully installed activeresource-3.0.0.rc
Successfully installed mime-types-1.16
Successfully installed polyglot-0.3.1
Successfully installed treetop-1.4.8
Successfully installed mail-2.2.5
Successfully installed actionmailer-3.0.0.rc
Successfully installed thor-0.14.0
Successfully installed railties-3.0.0.rc
Successfully installed bundler-1.0.0.rc.5
Successfully installed rails-3.0.0.rc
23 gems installed
{% endhighlight %}

If you read my previous [Rails 3 post](http://skim.la/2010/02/07/rails-3-beta-on-windows-using-rubyinstaller-187-rc2/), we had to install a lot of the dependencies separately.  The latest Rails gem seems to take care of it.  Also, RubyInstaller comes with Rake, so you do not need to install it.

Next up is installing SQLite or MySQL (your preference).

*Note:* The official Rails blog [mentions](http://weblog.rubyonrails.org/2010/7/26/rails-3-0-release-candidate) support for the MySQL2 gem which takes care of the MySQL encoding issues on Ruby 1.9.2.  At the time of this writing, I could not install MySQL2 gem on my Windows box, but the gem author seems to be [fully aware](http://github.com/brianmario/mysql2/issues#issue/8) of the situation.  In the meantime you should be able to use [SQLite](http://blog.mmediasys.com/2009/07/06/getting-started-with-rails-and-sqlite3/) or [MySQL with the mysql gem](http://blog.mmediasys.com/2009/07/06/getting-started-with-rails-and-mysql/).

Let's install SQLite:

{% highlight text %}
c:\devkit>gem install sqlite3-ruby
Temporarily enhancing PATH to include DevKit...

=============================================================================

  You've installed the binary version of sqlite3-ruby.
  It was built using SQLite3 version 3.6.23.1.
  It's recommended to use the exact same version to avoid potential issues.

  At the time of building this gem, the necessary DLL files where available
  in the following download:

  http://www.sqlite.org/sqlitedll-3_6_23_1.zip

  You can put the sqlite3.dll available in this package in your Ruby bin
  directory, for example C:\Ruby\bin

=============================================================================

Successfully installed sqlite3-ruby-1.3.1-x86-mingw32
1 gem installed
{% endhighlight %}

The instructions above says to use SQLite version 3.6.23.1 and provides a link to the dll.  I would also get the exe which is available [here](http://www.sqlite.org/sqlite-3_6_23_1.zip).  Place sqlite3.dll and sqlite3.exe in *C:\Ruby192\bin* or a general bin directory in the PATH (thanks for the correction, Luis!).

Now let's create a new Rails app.  Note, the new command to create the app:

{% highlight text %}
E:\p\rails>rails new rails3rc --database=sqlite3
      create
      create  README
      create  Rakefile
      create  config.ru
      create  .gitignore
      create  Gemfile
      create  app
      create  app/controllers/application_controller.rb
      create  app/helpers/application_helper.rb
      create  app/views/layouts/application.html.erb
      create  app/mailers
      create  app/models
      create  config
      create  config/routes.rb
      create  config/application.rb
      create  config/environment.rb
      create  config/environments
      create  config/environments/development.rb
      create  config/environments/production.rb
      create  config/environments/test.rb
      create  config/initializers
      create  config/initializers/backtrace_silencers.rb
      create  config/initializers/inflections.rb
      create  config/initializers/mime_types.rb
      create  config/initializers/secret_token.rb
      create  config/initializers/session_store.rb
      create  config/locales
      create  config/locales/en.yml
      create  config/boot.rb
      create  config/database.yml
      create  db
      create  db/seeds.rb
      create  doc
      create  doc/README_FOR_APP
      create  lib
      create  lib/tasks
      create  lib/tasks/.gitkeep
      create  log
      create  log/server.log
      create  log/production.log
      create  log/development.log
      create  log/test.log
      create  public
      create  public/404.html
      create  public/422.html
      create  public/500.html
      create  public/favicon.ico
      create  public/index.html
      create  public/robots.txt
      create  public/images
      create  public/images/rails.png
      create  public/stylesheets
      create  public/stylesheets/.gitkeep
      create  public/javascripts
      create  public/javascripts/application.js
      create  public/javascripts/controls.js
      create  public/javascripts/dragdrop.js
      create  public/javascripts/effects.js
      create  public/javascripts/prototype.js
      create  public/javascripts/rails.js
      create  script
      create  script/rails
      create  test
      create  test/performance/browsing_test.rb
      create  test/test_helper.rb
      create  test/fixtures
      create  test/functional
      create  test/integration
      create  test/unit
      create  tmp
      create  tmp/sessions
      create  tmp/sockets
      create  tmp/cache
      create  tmp/pids
      create  vendor/plugins
      create  vendor/plugins/.gitkeep
{% endhighlight %}

Start up the local server by running this command:

{% highlight text %}
E:\p\rails\rails3rc>rails server
=> Booting WEBrick
=> Rails 3.0.0.rc application starting in development on http://0.0.0.0:3000
=> Call with -d to detach
=> Ctrl-C to shutdown server
[2010-08-21 11:48:18] INFO  WEBrick 1.3.1
[2010-08-21 11:48:18] INFO  ruby 1.9.2 (2010-08-18) [i386-mingw32]
[2010-08-21 11:48:18] INFO  WEBrick::HTTPServer#start: pid=4296 port=3000
{% endhighlight %}

Now you should see that familiar **Welcome aboard** page on [http://localhost:3000/](http://localhost:3000/).

![Rails 3 RC](/images/rails3rc.jpg)

Now follow the [Rails 3 Guide](http://edgeguides.rubyonrails.org/) to build yourself a Rails 3 app!

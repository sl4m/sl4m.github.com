---
layout: post
title: rails 3 beta on windows using rubyinstaller 1.8.7 rc2
date: 2010-02-07 19:15:00 -08:00
categories:
  -- ruby
  -- rails
  -- windows
  -- rubyinstaller
---

Yesterday, I upgraded my Vista x64 machine to Windows 7 x64, so I needed to set up my Ruby/Rails environment again.  As done previously, I followed information on AkitaOnRails' blog post, [The Best Environment for Rails on Windows](http://akitaonrails.com/2009/1/13/the-best-environment-for-rails-on-windows) to install the latest [msysgit](http://code.google.com/p/msysgit/), Exuberant CTags, [gvim](http://www.vim.org/index.php), and [vimfiles](http://akitaonrails.com/2009/04/27/the-best-environment-for-rails-on-windows-part-2), replacing the deprecated One-Click Ruby Installer with the new [RubyInstaller 1.8.7 RC2](http://rubyforge.org/frs/?group_id=167&release_id=42563) that officially came out today.

*Requirements*

* [RubyInstaller 1.8.7 RC2](http://rubyforge.org/frs/download.php/69034/rubyinstaller-1.8.7-p249-rc2.exe)
* [DevKit 3.4.5r3-20091110](http://rubyforge.org/frs/download.php/66888/devkit-3.4.5r3-20091110.7z)
* [7-zip](http://www.7-zip.org/download.html) or zip program to uninstall 7z extensions
* [SQLite3](http://www.sqlite.org/sqlitedll-3_6_22.zip)

*Installation*

I chose C:\Ruby187\ as my default path and applied DevKit on top of it.  Be sure to follow the instructions in INSTALL.txt in the DevKit archive.  I placed sqlite3.dll in C:\Ruby187\bin.

I followed the instructions on Matt Hulse's post, [Massaging Rails 3 Beta on Windows](http://matt-hulse.com/articles/2010/02/05/massaging-rails-3-beta-on-windows/) to install the important gems for rails and created a quick rails 3 app.

{% highlight text %}
C:\Users\skim>gem install tzinfo builder memcache-client rack rack-test rack-mount erubis mail text-format thor bundler i18n rake --no-ri --no-rdoc
Successfully installed tzinfo-0.3.16
Successfully installed builder-2.1.2
Successfully installed memcache-client-1.7.8
Successfully installed rack-1.1.0
Successfully installed rack-test-0.5.3
Successfully installed rack-mount-0.4.5
Successfully installed abstract-1.0.0
Successfully installed erubis-2.6.5
Successfully installed activesupport-2.3.5
Successfully installed mime-types-1.16
Successfully installed mail-2.1.2
Successfully installed text-hyphen-1.0.0
Successfully installed text-format-1.0.0
Successfully installed thor-0.13.0
Due to a rubygems bug, you must uninstall all older versions of bundler for 0.9 to work
Successfully installed bundler-0.9.3
Successfully installed i18n-0.3.3
Successfully installed rake-0.8.7
17 gems installed  

C:\Users\skim>gem install rails --pre --no-ri --no-rdoc
Successfully installed activesupport-3.0.0.beta
Successfully installed activemodel-3.0.0.beta
Successfully installed actionpack-3.0.0.beta
Successfully installed arel-0.2.pre
Successfully installed activerecord-3.0.0.beta
Successfully installed activeresource-3.0.0.beta
Successfully installed actionmailer-3.0.0.beta
Successfully installed railties-3.0.0.beta
Successfully installed rails-3.0.0.beta
9 gems installed
{% endhighlight %}

![Rails 3 Beta](/images/rails3beta.jpg)

I recommend visiting [RubyInstaller Google Group](http://groups.google.com/group/rubyinstaller) if you have any problems with the RubyInstaller.
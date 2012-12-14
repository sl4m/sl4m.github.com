---
layout: post
title: phantomjs for headless capybara and jasmine
date: 2012-06-24 16:00:00 -05:00
tag: javascript
---

[phantomjs](http://phantomjs.org/) is one of the two solutions I'm aware of that solves the headless browser problem.  What makes phantomjs shine above the other is its lack for requiring [xvfb](http://en.wikipedia.org/wiki/Xvfb) and a browser like Firefox.  Starting with version 1.5.0, phantomjs de-coupled itself from xvfb and runs completely independent from it.  What does this mean for developers?  Fast test execution.  It no longer has to wait for Firefox to start up which takes seconds away from your execution.  Also, it runs JavaScript specific Cucumbers and Jasmine tests faster than the xvfb solution.

[Ariya Hidayat](https://github.com/ariya), the author of phantomjs has done a great job with this project.  It's super simple to set up on your Mac OS X, Linux, and Windows environment.  Simply [download](http://code.google.com/p/phantomjs/downloads/list) the appropriate binary, tar it to a directory, and symlink the phantomjs binary to a location in your `PATH`.  If you're on Mac OS X like me and have homebrew installed, then it's even easier.  `brew install phantomjs`.

Run `phantomjs` and you should see the REPL:

<pre><code class="no-highlight">
phantomjs> 1 + 1
2
phantomjs>
</code></pre>

There are a couple of more libraries that are required to run your Capybara-based Cucumbers and Jasmine tests.  [poltergeist](https://github.com/jonleighton/poltergeist) is the solution to run your Capybara-based Cucumber tests in a headless browser environment.  [guard-jasmine](https://github.com/netzpirat/guard-jasmine) does a great job running Jasmine tests and reporting the results nicely in your STDOUT.

Both repos have great READMEs that explain how to set it up, so I won't repeat them here. One thing you may want to do for the guard-jasmine set up is add a couple of rake tasks that mimic Jasmine's default rake task (starts up a Jasmine server) and ci task.

<pre><code class="ruby">
  namespace :jasmine do
    require 'jasmine'
    jasmine_config_override = File.join(File.dirname(__FILE__), 'spec', 'javascripts', 'support', 'jasmine_config.rb')
    require jasmine_config_override if File.exist?(jasmine_config_override)
    jasmine_config = Jasmine::Config.new
    ENV['JASMINE_PORT'] ||= '8888'
    guard_jasmine = "guard-jasmine spec --port=#{ENV['JASMINE_PORT']} --url=http://localhost:#{ENV['JASMINE_PORT']}/"

    desc "Starts Jasmine server"
    task :server do
      puts "your tests are here:"
      puts "  http://localhost:#{ENV['JASMINE_PORT']}"
      jasmine_config.start_server(ENV['JASMINE_PORT'])
    end

    desc "Runs specs via phantomjs against Jasmine server (used in conjunction with jasmine:server)"
    task :run do
      sh "#{guard_jasmine} --server=none"
    end

    desc "Starts Jasmine server and runs specs via phantomjs"
    task :ci do
      puts ""
      jasmine_config.start_jasmine_server
      sh guard_jasmine
    end
  end
</code></pre>

Note: guard-jasmine is very sensitive and will not work properly if you don't end your url option with a forward slash.  I'm not sure how long it took me to figure that out.

With these rake tasks in place, running `jasmine:server` will start the Jasmine server (via WEBrick) and running `jasmine:run` will run the Jasmine tests on the Jasmine server.  This will be handy when you are in development mode.  `jasmine:ci` works pretty much the same as the original `jasmine:ci` except now it runs through phantomjs and arguably prints a nicer output.

### Performance

I ran a quick, perhaps meaningless, performance test for Javascript-based Cucumber tests and Jasmine tests on both xvfb with Firefox and phantomjs.  I've wrapped test execution commands with `time`.

Here are the JavaScript-based Cucumber results:

<pre><code class="no-highlight">
# xvfb with Firefox

Test 1: 2:53.76
Test 2: 2:52.18
Test 3: 2:41.78

# phantomjs with poltergeist

Test 1: 44.185
Test 2: 43.890
Test 3: 46.311
</code></pre>

Approximately 73.5% faster with phantomjs, not bad, right?

<pre><code class="no-highlight">
# xvfb with Firefox

Test 1: 10.664
Test 2:  9.539
Test 3:  9.350
Test 4:  9.021
Test 5:  9.212

# phantomjs with guard-jasmine

Test 1:  4.604
Test 2:  3.986
Test 3:  3.963
Test 4:  3.877
Test 5:  4.007
</code></pre>

Approximately 57.1% faster with phantomjs.

As you can see, phantomjs is a dramatic improvement over the not-quite-true headless xvfb with Firefox.  Hopefully this convinces you to try out phantomjs whether you're coming from using xvfb or a head browser environment.  phantomjs is a welcoming addition to your development environment.

If you're a heavy Selenium user, be on a lookout for the new [ghostdriver](https://github.com/detro/ghostdriver/) that is based on phantomjs.

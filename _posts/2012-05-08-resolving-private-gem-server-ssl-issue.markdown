---
layout: post
title: resolving private gem server ssl issue
date: 2012-05-08 18:00:00 -05:00
tag: ruby
---

If you've been following the Ruby news, the Ruby core team [recently](http://www.ruby-lang.org/en/news/2012/04/20/ruby-1-9-3-p194-is-released/) [released](http://www.ruby-lang.org/en/news/2012/04/21/ruby-1-9-2-p320-is-released/) security fixes for RubyGems.  The fixes were:

* Turn on verification of server SSL certs
* Disallow redirects from https to http

We're going to focus on the first fix.  After upgrading to the latest patch, if you receive the following error message when attempting to install a gem from a private gem server:

````
Gem::RemoteFetcher::FetchError: SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed (https://gems.privateserver.com/gems/some-gem-1.0.0.gem)
````

then you'll need to do the following:

* determine which Root Certificate Authority (CA) you used for your private gem server
* view this [pem](http://curl.haxx.se/ca/cacert.pem) file (up-to-date certificate date from Mozilla via curl website)
* locate and copy the root certificate you care about and store it in a file
* save the file as a pem file (e.g., `some-name.pem`) - it can be saved anywhere
* open up `~/.gemrc` - if you don't have it, create it
* add the following:

`:ssl_ca_cert: /path/to/pem/file.pem`

You should now be able to `gem install` the desired gem from your private gem server.

---
layout: post
title: switching from pygments to highlight.js
date: 2012-06-17 21:00:00 -05:00
tag: javascript
---

There are a couple of JavaScript-based libraries out there that take a stab at syntax highlighting.  One of them is [Rainbow](https://github.com/ccampbell/rainbow) and the other is [highlight.js](https://github.com/isagalaev/highlight.js).

I really like the [Tomorrow Theme](https://github.com/chriskempson/tomorrow-theme) by Chris Kempson and attempted to create the Tomorrow Night theme for both Rainbow and highlight.js.  Craig, the author of Rainbow kindly accepted my [pull request](https://github.com/ccampbell/rainbow/pull/58), but the Tomorrow Theme on Rainbow is far from being complete.  However, because there aren't many customized options in Rainbow, I'm not sure if I can make it better than it is now.

The Tomorrow Night theme for highlight.js remains to be a work-in-progress.  The author has created a `test.html` that contains code examples of all the languages it supports.  This makes it easy to create the theme and make it work for the all of the languages.  At the same time, it's exhausting, so I have not completed it for all languages.  I'm using it for this blog and it should highlight properly for the languages that I use the most.

I decided to go with highlight.js because it has a lot more options to be able to replicate themes and language specific options.  To replace pygments with highlight.js, simply replace the begin and end tags:

<pre><code class="no-highlight">
From this

{&#37; highlight ruby &#37;}
klass = Class.new
{&#37; endhighlight &#37;}

To this

&lt;pre>&lt;code class="ruby">
klass = Class.new
&lt;/code>&lt;/pre>
</code></pre>

One thing I noticed when switching from pygments to highlight.js is the significant improvement in processing the blog.  When running `jekyll`, what used to take half a minute to a minute now takes a couple of seconds to run.  It looks like pygments is the bottleneck.

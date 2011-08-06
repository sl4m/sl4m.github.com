---
layout: post
title: gnu smalltalk koans
date: 2011-06-21 23:59:00 -05:00
categories:
  -- continuous learning
  -- koans
---

It's been a while since I blogged here, but I recently gave a talk, "Introduction to GNU Smalltalk" at [Chicago Code Camp](http://chicagocodecamp.com/sessions/114) and just moments ago, "GNU Smalltalk Koans" at the [Software Craftsmanship McHenry](http://mchenry.softwarecraftsmanship.org/#!/introduction-to-smalltalk-by-steve-kim) meetup.  Yes, you read that right.  A set of tests to teach you the Smalltalk language.  See slides [here](http://smalltalk.heroku.com/).

Heavily inspired by [Ruby Koans](http://skim.la/2010/03/29/ruby-koans-is-awesome) and [Clojure Koans](http://skim.la/2010/07/28/clojure-koans-is-awesome), [GNU Smalltalk Koans](https://github.com/sl4m/gnu_smalltalk_koans) attempts to teach you the Smalltalk language, syntax, and most of the standard libraries.  If you're a Rubyist and have not been exposed to Smalltalk, here's your chance to find out how much Smalltalk influenced Ruby.

The `README.markdown` should suffice in getting you started, but let's get down to the nitty gritty and reach our way to enlightenment.

## Requirements

* Mac OS X or Linux
* GNU Smalltalk `3.2.2` or higher
* an editor of your choice that supports Smalltalk (e.g., redcar, vim, emacs, Textmate)

*Note:* Unfortunately, Windows support is not available at this time.  I'm unable to build it on my system, but feel free to fork the project and contribute!

## Git Clone Project or Download tarball/zip

If you have git on your system, the easiest way to get the koans is to perform:

`git clone git@github.com:sl4m/gnu_smalltalk_koans.git`

If you don't have git, not a problem.  Go to the [Downloads](https://github.com/sl4m/gnu_smalltalk_koans/archives/master) section to grab your tarball or zip file and extract it to a location of your choosing.

## Running Koans

Once you're ready to dive in, simply type: `script/run` in the project root directory.  You will see a message like:

`TestAssert#testTruth has damaged your karma.`

It will tell you which file to open and which test to correct.  In this case, you'll need to open `TestAssert.st` file under `src/koans` and correct `testTruth` method (aka message).  Once you have corrected the test, re-run `script/run` to see what other koans might have damaged your karma.  Repeat and rinse.

## Closing

Be sure to read the rest of the `README.markdown` as it has some important information.  This is an open source project, so feel free to fork it, contribute, give feedback, I'm all ears!  Thanks for stopping by!

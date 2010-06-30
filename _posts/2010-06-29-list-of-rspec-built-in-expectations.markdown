---
layout: post
title: git branching and merging
date: 2010-06-29 20:30:00 -05:00
categories:
  -- ruby
  -- rspec
---

Here's the list of RSpec Built-in expectations straight off of David Chelimsky's [RSpec](http://www.pragprog.com/titles/achbd/the-rspec-book) book.

{% highlight ruby %}
actual.should equal(expected)
actual.should eql(expected)
actual.should == expected 

actual.should_not equal(expected)
actual.should_not eql(expected)
actual.should_not == expected

actual.should be_[predicate]
actual.should be_a_predicate
actual.should be_an_[predicate]

actual.should be_[predicate](*args)
actual.should be_a_[predicate](*args)
actual.should be_an_[predicate](*args)

actual.should_not be_[predicate]
actual.should_not be_a_[predicate]
actual.should_not be_an_[predicate]

actual.should_not be_[predicate](*args)
actual.should_not be_a_[predicate](*args)
actual.should_not be_an_[predicate](*args)

actual.should match(expected)
actual.should =~ expected

actual.should_not match(expected)
actual.should_not =~ expected

actual.should be < expected
actual.should be <= expected
actual.should be >= expected
actual.should be > expected

actual.should include(expected)
actual.should have(n).items
actual.should have_exactly(n).items
actual.should have_at_least(n).items
actual.should have_at_most(n).items

actual.should_not include(expected)
actual.should_not have(n).items
actual.should_not have_exactly(n).items

proc.should raise_error
proc.should raise_error(type)
proc.should raise_error(message)
proc.should raise_error(type, message)

proc.should_not raise_error
proc.should_not raise_error(type)
proc.should_not raise_error(message)
proc.should_not raise_error(type, message)

proc.should throw_symbol
proc.should throw_symbol(type)

proc.should_not throw_symbol
proc.should_not throw_symbol(type)

actual.should be_close(expected, delta)

actual.should_not respond_to(*messages)

actual.should satisfy { |actual| block }

actual.should_not satisfy { |actual| block }
{% endhighlight %}

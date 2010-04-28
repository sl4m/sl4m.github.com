---
layout: post
title: ruby koans is awesome
date: 2010-03-29 22:00:00 -07:00
categories:
  -- ruby
  -- continuous learning
---

[Ruby Koans](http://github.com/edgecase/ruby_koans) is a set of tests to teach you the Ruby language.  I'm impressed how much I'm learning from these tests and right off the bat, it's using Test::Unit which gives you an introduction to [TDD](http://en.wikipedia.org/wiki/Test-driven_development) style coding.

## How to get started

The README.rdoc at Ruby Koans [GitHub repo](http://github.com/edgecase/ruby_koans) should suffice, but if that wasn't clear, you should have the following already installed:

* Ruby 1.8.6 or higher (I used 1.8.7)
* msysgit, Git OSX, or zip program

Basically, do a git clone on the Ruby Koans repo (or download zip [file](http://github.com/downloads/edgecase/ruby_koans/rubykoans.zip)):

{% highlight text %}
git clone git://github.com/edgecase/ruby_koans.git
Initialized empty Git repository in E:/p/git/fake/ruby_koans/.git/
remote: Counting objects: 294, done.
remote: Compressing objects: 100% (286/286), done.
remote: Total 294 (delta 184), reused 0 (delta 0)
Receiving objects: 100% (294/294), 51.84 KiB, done.
Resolving deltas: 100% (184/184), done.
{% endhighlight %}

Done.  Get out your favorite editor (or IDE) and have a command prompt|shell or two ready.  You want to first run *path\_to\_enlightenment.rb*:

{% highlight text %}
ruby path_to_enlightenment.rb

Thinking AboutAsserts
  test_assert_truth has damaged your karma.

You have not yet reached enlightenment ...
<false> is not true.

Please meditate on the following code:
./about_asserts.rb:10:in `test_assert_truth'
path_to_enlightenment.rb:28


mountains are merely mountains
{% endhighlight %}

Open *about\_asserts.rb* and look up the test method called *test\_assert\_truth* and fix whatever is wrong with it.  The tests will give you hints, so don't worry, you'll figure it out.  Once you do, save the file, re-run *path\_to\_enlightenment.rb*.

{% highlight text %}
ruby path_to_enlightenment.rb

Thinking AboutAsserts
  test_assert_truth has expanded your awareness.
  test_assert_with_message has damaged your karma.

You have not yet reached enlightenment ...
This should be true -- Please fix this.
<false> is not true.

Please meditate on the following code:
./about_asserts.rb:16:in `test_assert_with_message'
path_to_enlightenment.rb:28


learn the rules so you know how to break them properly
{% endhighlight %}

You will now see that *test\_assert\_truth* has expanded your awareness.  Now go on to the next test and fix the problem in there.  Rinse and repeat.

If you ever get up to *about\_scoring\_project.rb*, please let me know how you implemented the score method.  Here's mine:

{% highlight ruby %}
def score(dice)
  total = 0
  rolls = dice.dup
  return total if rolls.size == 0
  (1..6).each do |i| 
    if rolls.count(i) >= 3
      3.times do
        rolls.delete_at(rolls.index(i))
      end
      total += (i == 1) ? 1000 : 100*i
    end
  end
  rolls.each do |i|
    if i == 1
      total += 100
    elsif i == 5
      total += 50
    end
  end
  total
end
{% endhighlight %}

I added additional tests just for the heck of it:

{% highlight ruby %}
  def test_other_oddities
    assert_equal 1100, score([1,1,1,1])
    assert_equal 1200, score([1,1,1,1,1])
    assert_equal 600, score([5,5,5,5,5])
  end
{% endhighlight %}

Here's my Proxy class from *about\_proxy\_object\_project.rb*:

{% highlight ruby %}
class Proxy
  attr_reader :messages

  def initialize(target_object)
    @object = target_object
    @messages = []
  end

  def method_missing(method_name, *args, &block)
    @messages.push(method_name)
    @object.__send__(method_name, *args)
  end

  def called?(message)
    return true unless @messages.index(message).nil?
    false
  end

  def number_of_times_called(message)
    return @messages.count(message)
  end
end
{% endhighlight %}

Have fun!
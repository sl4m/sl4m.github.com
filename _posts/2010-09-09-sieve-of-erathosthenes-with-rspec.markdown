---
layout: post
title: sieve of eratosthenes with rspec
date: 2010-09-09 11:00:00 -07:00
categories:
  -- ruby
  -- continuous learning
---

Four months ago, I [wrote Sieve of Eratosthenes algorithm in Ruby](http://skim.la/2010/05/09/sieve-of-eratosthenes/).  Today, I wanted to try it again with RSpec tests and a kata-like style.  You'll notice the test is inspired by Uncle Bob's kata test for [Prime Factors](http://vimeo.com/7762511).

## sieve\_spec.rb

{% highlight ruby %}
$: << File.expand_path(File.dirname(__FILE__))
require 'spec'
require 'sieve'

describe Sieve do
  [
    [1,   []],
    [2,   [2]],
    [3,   [2,3]],
    [4,   [2,3]],
    [5,   [2,3,5]],
    [6,   [2,3,5]],
    [7,   [2,3,5,7]],
    [8,   [2,3,5,7]],
    [9,   [2,3,5,7]],
    [10,  [2,3,5,7]],
    [11,  [2,3,5,7,11]],
    [13,  [2,3,5,7,11,13]],
    [17,  [2,3,5,7,11,13,17]],
    [19,  [2,3,5,7,11,13,17,19]],
    [23,  [2,3,5,7,11,13,17,19,23]],
    [29,  [2,3,5,7,11,13,17,19,23,29]],
    [100, [2,3,5,7,11,13,17,19,23,29,31,37,41,43,47,53,59,61,67,71,73,79,83,89,97]]
  ].each do |number, primes|
    it "returns prime numbers up to #{number}" do
      Sieve.of(number).should == primes
    end
  end
end
{% endhighlight %}

## sieve.rb

{% highlight ruby %}
module Sieve
  def self.of(n)
    return [] if n <= 1
    not_primes = []
    (2..n).each do |num|
      if num <= Math.sqrt(n)
        (2..n).each do |p|
          not_primes << p if num != p && p % num == 0
        end
      end
    end
    return (2..n).to_a - not_primes
  end
end
{% endhighlight %}

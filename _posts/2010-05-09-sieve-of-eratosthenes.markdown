---
layout: post
title: sieve of eratosthenes
date: 2010-05-09 13:00:00 -07:00
categories:
  -- ruby
  -- continuous learning
---

*Update:* Four months later, I found a bug in my code.  *if m < Math.sqrt(@number)* should be *if m <= Math.sqrt(@number)*

Last week, I learned about Sieve of Eratosthenes.  According to [Wikipedia](http://en.wikipedia.org/wiki/Sieve_of_Eratosthenes), it's an ancient algorithm created by an ancient Greek mathematician, Eratosthenes, for finding all prime numbers (below 10 million).

I was asked to write the ancient algorithm in Ruby.  Here is the code.

{% highlight ruby %}
class Sieve
  attr_reader :prime_numbers

  def initialize(number)
    @number = number.to_i
    @prime_numbers = (2..@number).to_a
    remove_non_prime_numbers
  end

  def remove_non_prime_numbers
    @prime_numbers.each do |m|
      if m <= Math.sqrt(@number)
        @prime_numbers.each do |n|
          if m != n && n % m == 0
            @prime_numbers.delete(n)
          end
        end
      end
    end
  end
end

if $0 == __FILE__
  p = Sieve.new(ARGV[0]).prime_numbers
  puts 'There are ' + p.length.to_s + ' prime numbers: ' + p.join(', ')
end
{% endhighlight %}

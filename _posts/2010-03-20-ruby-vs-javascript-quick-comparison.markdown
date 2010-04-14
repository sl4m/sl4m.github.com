---
layout: post
title: ruby vs. javascript - quick comparison
date: 2010-03-20 20:30:00 -07:00
categories:
  -- ruby
  -- javascript
---

These are my initial observations between the two languages so far.

## nil and null

*Ruby*
{% highlight ruby %}
nil.is_a?(Object) # true
{% endhighlight %}
*JavaScript*
{% highlight javascript %}
typeof null === "object" // true
{% endhighlight %}

Unlike Ruby, *null* is not derived from a class (or object), but is a special value, so it does not have any members.  *typeof* returns "object" for backward compatibility reasons.

## falsy values

In Ruby, zero and empty strings are considered truthy values.

*Ruby*
{% highlight ruby %}
status = (nil) ? true : false   # status = false
status = (false) ? true : false # status = false
{% endhighlight %}
*JavaScript*
{% highlight javascript %}
var status;
status = (null) ? true : false;      // status = false
status = (false) ? true : false;     // status = false
status = (0) ? true : false;         // status = false
status = ("") ? true : false;        // status = false
status = ('') ? true : false;        // status = false
status = (undefined) ? true : false; // status = false
{% endhighlight %}

## strings

Both Ruby and JavaScript use single and double quotes for creating strings.  You can also use single quotes in double quotes and vice versa without the need to escape them.  However, Ruby has some powerful flexible quoting features.

*Ruby*
{% highlight ruby %}
a = %(hello world)  # "hello world"
b = %!hello world!  # "hello world"
c = %{hello world}  # "hello world"
d = %Q{hello world} # "hello world"
e = %{
hello
world
}                   # "\nhello\nworld\n"
{% endhighlight %}

Ruby uses the shovel operator to modify original strings.

*Ruby*
{% highlight ruby %}
string = "hello"
new_string = string
new_string << " world"  # "hello world" (appends ' world' to new_string)
string == "hello world" # true (modified original string)
{% endhighlight %}

Ruby does not interpret escape characters when using single quotes (except itself).  JavaScript always interprets.

*Ruby*
{% highlight ruby %}
"\n".size   # 1
'\n'.size   # 2
'\''        # "'"
{% endhighlight %}
*JavaScript*
{% highlight javascript %}
"\n".length // 1
'\n'.length // 1
'\''        // "'"
{% endhighlight %}

Ruby has some other features.

*Ruby*
{% highlight ruby %}
number = 21
string = "Your lucky number today is #{number}" # "Your lucky number today is 21"
                                                # (string interpolation only works for double quoted strings)

string[5,5]  # 'lucky' (substring)
string[5..9] # 'lucky'
{% endhighlight %}

## arrays

Ruby has some really cool shorthand methods.

*Ruby*
{% highlight ruby %}
array = []
array << 'a'                  # shovel operator acts like JavaScript's push method
array[1] = 'b'
array.push('c')
array.size                    # 3 (Ruby also has length like JavaScript)
array.first                   # "a" (first element)
array.last                    # "c" (last element)
array[-1]                     # "c" (walks backwards)
array[-3]                     # "a"
array[0,1]                    # ["a"] (acts like JavaScript's slice method)
array[1,2]                    # ["b", "c"]
array[0..2]                   # ["a", "b", "c"] (between index 0 and 2, inclusive)
first, second, third = array  # first => "a", second => "b", third => "c"
another_array = %w(a b c)     # ["a", "b", "c"] (equivalent to String#split)
array == another_array        # true
array - %w(a b)               # ["c"] (returns the difference between both arrays)
{% endhighlight %}

##Resources

* [Ruby Koans](http://github.com/edgecase/ruby_koans)
* [Ruby Shortcuts](http://caiustheory.com/ruby-shortcuts)
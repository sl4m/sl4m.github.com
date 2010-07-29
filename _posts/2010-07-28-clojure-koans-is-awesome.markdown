---
layout: post
title: clojure koans is awesome
date: 2010-07-28 21:30:00 -05:00
categories:
  -- clojure
  -- koans
---

I attended my first [Chicago Clojure](http://www.meetup.com/Chicago-Clojure/) meetup and 8th Light's [Colin Jones](http://twitter/com/trptcolin) introduced us to Clojure Koans, a Ruby Koans inspired set of tests to teach you [Clojure](http://clojure.org/), a functional programming language.  After finishing the koans that night, I have a better understanding of how Clojure syntax works.  You might have heard Clojure uses a lot of parentheses - it's probably not an overstatement.

Here's how to get started and reach your way to enlightenment.

## Installation

Clojure Koans is on a branch of [Functional Koans](http://github.com/relevance/functional-koans).  Each branch on this repo is using a different language.  First let's do a *git clone*:

{% highlight text %}
[~/local/git] git clone http://github.com/relevance/functional-koans.git
Cloning into functional-koans...
remote: Counting objects: 536, done.
remote: Compressing objects: 100% (498/498), done.
remote: Total 536 (delta 244), reused 0 (delta 0)
Receiving objects: 100% (536/536), 162.00 KiB | 90 KiB/s, done.
Resolving deltas: 100% (244/244), done.
[~/local/git] cd functional-koans 
[master][~/local/git/functional-koans]
{% endhighlight %}

If you take a look at the directory, you'll see there are only a couple of files:

{% highlight text %}
[master][~/local/git/functional-koans] ls -lah
total 16
drwxr-xr-x   5 skim  staff   170B Jul 28 22:07 ./
drwxr-xr-x  25 skim  staff   850B Jul 28 22:07 ../
drwxr-xr-x  13 skim  staff   442B Jul 28 22:07 .git/
-rw-r--r--   1 skim  staff   480B Jul 28 22:07 README.markdown
-rw-r--r--   1 skim  staff   497B Jul 28 22:07 ideaboard.txt
{% endhighlight %}

Again this is because Clojure Koans is in a branch of this repo.  You can read more about branching [here](http://skim.la/2010/06/28/git-branching-and-merging).  To switch to Clojure Koans, run this command:

{% highlight text %}
[master][~/local/git/functional-koans] git checkout -b clojure origin/clojure
Branch clojure set up to track remote branch clojure from origin.
Switched to a new branch 'clojure'
[clojure][~/local/git/functional-koans] ls -lah
total 64
drwxr-xr-x  13 skim  staff   442B Jul 28 22:12 .git/
-rw-r--r--   1 skim  staff    24B Jul 28 22:12 .gitignore
-rw-r--r--   1 skim  staff   676B Jul 28 22:12 README.md
-rw-r--r--   1 skim  staff   172B Jul 28 22:12 ideaboard.txt
-rw-r--r--   1 skim  staff   152B Jul 28 22:12 project.clj
-rwxr-xr-x   1 skim  staff   149B Jul 28 22:12 run.bat*
-rwxr-xr-x   1 skim  staff   142B Jul 28 22:12 run.sh*
drwxr-xr-x   5 skim  staff   170B Jul 28 22:12 src/
-rwxr-xr-x   1 skim  staff   147B Jul 28 22:12 test.bat*
-rwxr-xr-x   1 skim  staff   140B Jul 28 22:12 test.sh*
{% endhighlight %}

Cool, we're switched to the Clojure branch.  We're not done yet.  Create a directory called lib.

{% highlight text %}
[clojure][~/local/git/functional-koans] mkdir lib
{% endhighlight %}

Let's download the jar file which needs to be placed in this new lib directory.  Go the GitHub repo [download page](http://github.com/relevance/functional-koans/downloads).  Pick the latest if clojure-1.2.0-beta1.jar is not the latest.  Once the download is complete, move it to the lib directory.

{% highlight text %}
[clojure][~/local/git/functional-koans] ls -lah lib
total 6320
drwxr-xr-x   3 skim  staff   102B Jul 28 22:18 ./
drwxr-xr-x  13 skim  staff   442B Jul 28 22:14 ../
-rw-r--r--   1 skim  staff   3.1M Jul 27 20:54 clojure-1.2.0-beta1.jar
{% endhighlight %}

## Running Clojure Koans

Ok, now I'm not sure how this works exactly on Windows, but Mac and Linux, you should be able to run koans this way:

{% highlight text %}
[clojure][~/local/git/functional-koans] ./run.sh

FAIL in clojure.lang.PersistentList$EmptyList@1 (equalities.clj:1)
We shall contemplate truth by testing reality, via equality.
expected: (= __ true)
  actual: (not (= nil true))
{% endhighlight %}

Great!  If you read through the error message, you'll see there's something wrong in the equalities.clj file.  All files live in the src/koans directory.  Open it up, fix the tests and re-run the previous command.  For the sake of showing you an example, I fixed the first test:

{% highlight text %}
[clojure][~/local/git/functional-koans] cat src/koans/equalities.clj 
(meditations
  "We shall contemplate truth by testing reality, via equality."
  (= true true)

  "To understand reality, we must compare our expectations against reality."
  (= __ (+ 1 1))

  "You can test equality of many things"
  (= (+ 3 4) __ (+ 2 __)))
[clojure][~/local/git/functional-koans] ./run.sh                 

FAIL in clojure.lang.PersistentList$EmptyList@1 (equalities.clj:1)
To understand reality, we must compare our expectations against reality.
expected: (= __ (+ 1 1))
  actual: (not (= nil 2))
{% endhighlight %}

Now fix the next test and you'll be on your way to enlightenment.

## functions.clj

If you ever get to *functions.clj*, tell me how you implemented the last one!  Here's mine:

{% highlight clojure %}
  "Higher-order functions take function arguments"
  (= 25 ((fn [n f] (f n)) 5
          (fn [n] (* n n)))))
{% endhighlight %}

Have fun!

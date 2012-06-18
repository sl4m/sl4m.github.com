---
layout: post
title: clojure koans is awesome
date: 2010-07-28 21:30:00 -05:00
categories:
  -- clojure
  -- koans
---

*Update:* The Clojure Koans GitHub repo [moved](https://github.com/functional-koans/clojure-koans).  Instructions below have been updated.

I attended my first [Chicago Clojure](http://www.meetup.com/Chicago-Clojure/) meetup and 8th Light's [Colin Jones](http://twitter.com/trptcolin) introduced us to Clojure Koans, a Ruby Koans inspired set of tests to teach you [Clojure](http://clojure.org/), a functional programming language.  After finishing the koans that night, I have a better understanding of how Clojure syntax works.  You might have heard Clojure uses a lot of parentheses - it's probably not an overstatement.

Here's how to get started and reach your way to enlightenment.

## Installation

Clojure Koans is on [GitHub](https://github.com/functional-koans/clojure-koans).  First let's do a *git clone*:

<pre><code class="no-highlight">
[~/local/git] git clone https://github.com/functional-koans/clojure-koans.git
Cloning into clojure-koans...
remote: Counting objects: 487, done.
remote: Compressing objects: 100% (210/210), done.
remote: Total 487 (delta 264), reused 487 (delta 264)
Receiving objects: 100% (487/487), 60.71 KiB, done.
Resolving deltas: 100% (264/264), done.
[~/local/git] cd clojure-koans
[master][~/local/git/clojure-koans]
</code></pre>

Here's a quick look at the directory.  *Note:* Clojure Koans is no longer a branch of Functional Koans.

<pre><code class="no-highlight">
[master][~/local/git/clojure-koans] ls -lah
total 64
drwxr-xr-x  10 skim  staff   340B Jan  2 12:49 ./
drwxr-xr-x  55 skim  staff   1.8K Jan  2 12:49 ../
drwxr-xr-x  13 skim  staff   442B Jan  2 12:49 .git/
-rw-r--r--   1 skim  staff    32B Jan  2 12:49 .gitignore
-rw-r--r--   1 skim  staff   3.7K Jan  2 12:49 README.md
-rw-r--r--   1 skim  staff    13K Jan  2 12:49 epl-v10.html
-rw-r--r--   1 skim  staff   504B Jan  2 12:49 ideaboard.txt
-rw-r--r--   1 skim  staff   165B Jan  2 12:49 project.clj
drwxr-xr-x  10 skim  staff   340B Jan  2 12:49 script/
drwxr-xr-x   5 skim  staff   170B Jan  2 12:49 src/
</code></pre>

Cool, now let's install [Leiningen](http://github.com/technomancy/leiningen) (pronounced 'LINE-ing-en') to grab the latest Clojure jar.  We'll need this jar along with JRE 1.5 or higher.  Most Macs should have JRE installed by default.

To install Leiningen, simply download this [script](https://github.com/technomancy/leiningen/raw/stable/bin/lein), place it in your $PATH (e.g., ~/bin) and chmod it.

I placed my copy in *~/bin* and ran chmod on it.

<pre><code class="no-highlight">
[~/bin] ls -lah
total 48
-rw-r--r--@ 1 skim  staff   6.0K Jun 19  2010 .DS_Store
-rwxr-xr-x  1 skim  staff   221B Jun 19  2010 ack*
-rw-r--r--@ 1 skim  staff   5.6K Jan  2 13:05 lein
-rwxr-xr-x  1 skim  staff   2.2K Jun 19  2010 mvim*
[~/bin] chmod 755 lein
[~/bin] ls -lah
total 48
-rw-r--r--@ 1 skim  staff   6.0K Jun 19  2010 .DS_Store
-rwxr-xr-x  1 skim  staff   221B Jun 19  2010 ack*
-rwxr-xr-x@ 1 skim  staff   5.6K Jan  2 13:05 lein*
-rwxr-xr-x  1 skim  staff   2.2K Jun 19  2010 mvim*
</code></pre>

Now let's run the lein executable.

<pre><code class="no-highlight">
[~/bin] ./lein
Downloading Leiningen now...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 6794k  100 6794k    0     0  1780k      0  0:00:03  0:00:03 --:--:-- 2905k
Leiningen is a build tool for Clojure.

Several tasks are available:
classpath   Show the classpath of the current project.
clean       Remove compiled artifacts and jars from project.
compile     Compile Clojure source into .class files.
deps        Download all dependencies and place them in the :library-path.
help        Display a list of tasks or help for a given task.
install     Install the current project or download the project specified.
interactive Enter interactive shell for calling tasks without relaunching JVM.
jar         Package up all the project's files into a jar file.
javac       Compile Java source files.
new         Create a new project skeleton.
plugin      Manage user-level plugins.
pom         Write a pom.xml file to disk for Maven interop.
repl        Start a repl session either with the current project or standalone.
run         Run a -main function with optional command-line arguments.
test        Run the project's tests.
test!       Run a project's tests after cleaning and fetching dependencies.
uberjar     Package up all the project's files and dependencies into a jar file.
upgrade     Upgrade Leiningen to the latest stable release.
version     Print version for Leiningen and the current JVM.

Run lein help $TASK for details.
Also available: readme, tutorial, copying, sample, and news.
</code></pre>

Excellent, we have Leiningen installed.  Now let's go back to the clojure-koans directory and run *lein deps*.  This will read the *project.clj* file to find out which dependencies it needs to install.

<pre><code class="no-highlight">
[master][~/local/git/clojure-koans] lein deps
Downloading: org/clojure/clojure/1.3.0-alpha3/clojure-1.3.0-alpha3.pom from central
Downloading: org/clojure/clojure/1.3.0-alpha3/clojure-1.3.0-alpha3.pom from clojure
Transferring 1K from clojure
Downloading: jline/jline/0.9.94/jline-0.9.94.pom from central
Downloading: junit/junit/3.8.1/junit-3.8.1.pom from clojure
Downloading: junit/junit/3.8.1/junit-3.8.1.pom from clojure-snapshots
Downloading: junit/junit/3.8.1/junit-3.8.1.pom from clojars
Downloading: junit/junit/3.8.1/junit-3.8.1.pom from central
Downloading: org/clojure/clojure/1.3.0-alpha3/clojure-1.3.0-alpha3.jar from central
Downloading: org/clojure/clojure/1.3.0-alpha3/clojure-1.3.0-alpha3.jar from clojure
Transferring 3528K from clojure
Downloading: jline/jline/0.9.94/jline-0.9.94.jar from central
Downloading: junit/junit/3.8.1/junit-3.8.1.jar from clojure
Downloading: junit/junit/3.8.1/junit-3.8.1.jar from clojure-snapshots
Downloading: junit/junit/3.8.1/junit-3.8.1.jar from clojars
Downloading: junit/junit/3.8.1/junit-3.8.1.jar from central
Copying 3 files to /Users/skim/local/git/clojure-koans/lib
</code></pre>

## Running Clojure Koans

Ok, now I'm not sure how this works exactly on Windows, but Mac and Linux, you should be able to run koans this way:

<pre><code class="no-highlight">
[master][~/local/git/clojure-koans] script/run

FAIL in clojure.lang.PersistentList$EmptyList@1 (equalities.clj:1)
We shall contemplate truth by testing reality, via equality.
expected: (= __ true)
  actual: (not (= nil true))
</code></pre>

Great!  If you read through the error message, you'll see there's something wrong in the equalities.clj file.  All files live in the src/koans directory.  Open it up, fix the tests and re-run the previous command.  For the sake of showing you an example, I fixed the first test:

<pre><code class="no-highlight">
[master][~/local/git/clojure-koans] cat src/koans/equalities.clj
(meditations
  "We shall contemplate truth by testing reality, via equality."
  (= true true)

  "To understand reality, we must compare our expectations against reality."
  (= __ (+ 1 1))

  "You can test equality of many things"
  (= (+ 3 4) __ (+ 2 __)))

[master][~/local/git/clojure-koans] script/run

FAIL in clojure.lang.PersistentList$EmptyList@1 (equalities.clj:1)
To understand reality, we must compare our expectations against reality.
expected: (= __ (+ 1 1))
  actual: (not (= nil 2))
</code></pre>

Now fix the next test and you'll be on your way to enlightenment.

## functions.clj

If you ever get to *functions.clj*, tell me how you implemented the last one!  Here's mine:

<pre><code class="lisp">
  "Higher-order functions take function arguments"
  (= 25 ((fn [n f] (f n)) 5
          (fn [n] (* n n)))))
</code></pre>

Have fun!

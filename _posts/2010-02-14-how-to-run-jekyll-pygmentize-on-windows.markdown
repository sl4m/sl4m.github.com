---
layout: post
title: how to run jekyll's pygmentize on windows
date: 2010-02-14 14:00:00 -08:00
categories:
  -- jekyll
  -- github
  -- pygmentize
  -- python
  -- windows
---

This is a quick patch created by [Jon](http://github.com/jonforums) at [RubyInstaller Google Group](http://groups.google.com/group/rubyinstaller/t/400ecfe5d528b558).

*What You Need*

* Any [RubyInstaller MinGW version](http://rubyinstaller.org/download.html) or [1.8.7/1.9.1 mswin32](http://www.ruby-lang.org/en/downloads/) (Not tested using One-Click Installer)
* [jekyll gem](http://gemcutter.org/gems/jekyll)
* [Python](http://www.python.org/download/) 2.3 or higher (I used 2.6.4)
* [setuptools 0.6c11](http://pypi.python.org/pypi/setuptools) for easy_install
* [pygments](http://pygments.org/) (install with easy_install)

*Installation*

Installing Ruby, jekyll, and Python should be self explanatory.  I installed RubyInstaller 1.8.7 RC2 to C:\Ruby187\ and Python 2.6.4 to C:\Python26\\. 

Modify the code in albino.rb and highlight.rb from C:\Ruby187\lib\ruby\gems\1.8\gems\jekyll-0.5.7\lib\jekyll\ using the following gists:

* [albino.rb](http://gist.github.com/304185)
* [highlight.rb (under tags folder)](http://gist.github.com/304187)

Be sure not to just replace the entire code, just the portions of the code.  If you're using Ruby 1.9.1 or your Ruby path is different, drill down to the corresponding path.

I created a folder called 'Scripts' under C:\Python26.  I added C:\Python26 and C:\Python26\Scripts to the PATH environment variable.

In order to install pygments, you need to install easy_install which is included in setuptools.  Download the source ([setuptools-0.6c11.tar.gz](http://pypi.python.org/packages/source/s/setuptools/setuptools-0.6c11.tar.gz#md5=7df2a529a074f613b509fb44feefe74e)) and egg ([setuptools-0.6c11-py2.6.egg](http://pypi.python.org/packages/2.6/s/setuptools/setuptools-0.6c11-py2.6.egg#md5=bfa92100bd772d5a213eedd356d64086)).  Extract ez_setup.py from setuptools-0.6.c11.tar.gz to the same folder as setuptools-0.6c11-py2.6.egg.  Open up a command prompt, navigate to the folder with the egg file and run:

{% highlight text %}
python ez_setup.py setuptools-0.6c9-py2.6.egg
{% endhighlight %}

This will add easy_install.exe and other files to your C:\Python26\Scripts folder.

Now to install pygments, simply run this command:

{% highlight text %}
easy_install Pygments
{% endhighlight %}

According to Jon, easy_install does not create a wrapper for the Pygmentize script, so creating a batch file pygmentize.bat under C:\Python26\Scripts should do the trick.  Add this command to the batch file:

{% highlight text %}
@python.exe %~dp0pygmentize %*
{% endhighlight %}

Now when running jekyll locally, you should be able to see the pygmentize highlights

![pygmentize on windows](/images/jekyll_pygmentize.jpg)
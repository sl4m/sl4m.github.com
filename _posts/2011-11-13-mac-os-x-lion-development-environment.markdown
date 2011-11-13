---
layout: post
title: mac os x lion development environment
date: 2011-11-13 15:00:00 -05:00
categories:
  -- environment
---

I finally bit the bullet and installed Lion on my Macbook Pro.  Mac OS X Lion released almost four months ago and comes with
 some interesting features.  You can read about them in detail [here](http://en.wikipedia.org/wiki/Mac_OS_X_Lion).
 What I'm going to write about here are some of the hurdles I had to overcome. Perhaps when it's time for you to upgrade,
 you won't have to experience them.  Of course, YMMV.

TL;DR

* Time Machine does not work if FileVault is enabled
* Use encrypted disk images or TrueCrypt instead of FileVault
* Lion Disk Utility detects if hard drive has too many [S.M.A.R.T.](http://en.wikipedia.org/wiki/S.M.A.R.T.) errors and simply refuses to install Lion if so
* Had to replace hard drive (should have purchased SSD, c'est la vie)
* XCode 4.2 is not rvm/homebrew friendly.  Have to use [XCode 4.1](https://developer.apple.com/downloads/download.action?path=Developer_Tools/xcode_4.1_for_lion/xcode_4.1_for_lion.dmg)
* XCode 4.1 installer installs `Install XCode.app` in your Applications folder. You then have to install `Install XCode.app` to install XCode. Not confusing at all.
* Homebrew does not have a vim formula (not talking about macvim). Have to compile on your own.

## Getting Ready

I could have easily upgraded to Lion from Snow Leopard, but I wanted a clean slate.  I collected a lot of cruft over the year
 and a half and I thought it would be educationally rewarding to set up the development environment again, but this time
 without a guide.  I also wanted a clean [dot-file](http://en.wikipedia.org/wiki/Dot-file) slate, learn about the commands in
 each dot-file instead of just accepting them and taking them for granted.

If you already have Time Machine set up (or similar backup software), then you're already ahead of the game.  I did not have
 any backup software installed/enabled (hard drives can fail???), so I manually copied directories and files over to my home
 NAS.  I didn't have a lot of stuff that I cared to back up, so this was a rather easy process.

## Lion on USB

Since I planned to wipe out the hard drive and start fresh, I needed to store the Lion installer on a USB thumb drive.  Apple 
 offers a USB thumb drive option, but why pay $69 when you can do it for free?  There are many guides out there with
 detailed instructions on how to extract the core file from Lion installer and transfer it to your USB thumb drive.

## Let's Install

I rebooted the computer, inserted the thumb drive, and held down the Option key.  I proceeded to the Disk Utility and wiped
 out my hard drive.  Then when I attempted to install Mac OS X Lion, I was greeted with S.M.A.R.T. errors message.  If you
 can't select the hard drive to tell the Lion installer the destination of your install, then chances are, you have S.M.A.R.T.
 errors.  I googled this issue and lots of people have reported the same issue.  The hard drive worked fine in Snow Leopard,
 Lion does not want to do anything with it.

I ordered a new hard drive from Amazon.com, waited a day for it to arrive, then tried again.  No S.M.A.R.T. errors.  BOOM, it
 let me proceed and within 15 minutes, Lion was on my machine.

## Post Install

First thing I did was check for Software Updates because I was well aware of the updates that had come in post Lion release
 date.  After that, I proceeded to install software I care about (in alphabetical order):

* Briquette (Propane alternative for Campfire)
* Chrome
* iTerm
* Silverlight (Netflix)
* SizeUp (window sizing tool)
* Skype
* VLC
* VMWare Fusion
* Vox

## Development Environment

The following tools were installed to aid my development needs:

* rvm (yes, I know about rbenv)
* homebrew
* XCode 4.1 (4.2 is evil)
* git
* vim (not macvim)
* [Tomorrow Theme](https://github.com/ChrisKempson/Vim-Tomorrow-Theme) for vim
* [vimfiles](https://github.com/braintreeps/vim_dotfiles)
* ctags (for vim)
* zsh
* [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting) (fish shell-like support for zsh)
* tmux

I know some hate rvm and prefer rbenv, but I'm fine with it.  I installed homebrew next.  Homebrew requires XCode, and I knew
 that beforehand; what I didn't know about is that it doesn't like XCode 4.2.  The App Store only has version 4.2 (and higher
 when you read this post at a later date).  The homebrew notes has the [link](https://developer.apple.com/downloads/download.action?path=Developer_Tools/xcode_4.1_for_lion/xcode_4.1_for_lion.dmg)
 to version 4.1.  Download and install.  Here's a caveat: the dmg contains an installer.  The installer installs
 `Install XCode.app` in your Applications folder.  You'll need to run that installer to finally get XCode installed.
 This confused me to no end for an hour.

Git, ctags, zsh, and tmux installations were straight forward.  Homebrew is your friend.  Unfortunately though, homebrew does
 not have a vim formula (and probably does not plan to).  <strike>I had to install this from scratch.  This is probably the
 long-winded process of getting it installed, but I digress:</strike> Update: You can brew install it.

{% highlight text %}
brew install https://raw.github.com/adamv/homebrew-alt/master/duplicates/vim.rb
{% endhighlight %}

I have been using MacVim for the past year and a half, but since this summer have been using it exclusively.  I'm taking a
 step further and switch to vim, so I can use it in tmux sessions, over ssh, etc.  I used to use AkitaOnRails'
 vimfiles, but decided to adopt this [vimfiles](https://github.com/braintreeps/vim_dotfiles) instead.  I'm using this
 setup right now to write this blog post and so far I'm loving it.

Caveats to getting the vimfiles to work.  If you're a fan of wincent's Command-T, then this may be troublesome.  The
 plugin kept segfaulting, so I removed it completely from the bundle directory.  This is even after compiling Command-T
 using system ruby.  The vimfiles uses FuzzyFinder anyway, so it's not the end of the world.

The vim that came with Lion will not work with vim plugins that need Ruby support.  You can easily check this by checking
 the version (`vim --version`).  It should have a plus next to `ruby`.  So if FuzzyFinder is not working for you, it's
 because `ruby` was not enabled for the vim you have.

## Final Words

Hope you enjoyed the TL;DR post.  I know I wasn't very descriptive on every step of the process, so if you have any questions,
 let me know in the comments.

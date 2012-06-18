---
layout: post
title: how to make github and proxy play nicely with ssh
date: 2010-02-22 22:00:00 -08:00
categories:
  -- github
  -- proxy
  -- ssh
---

*Update (06/30/2010)*: This method may no longer work with the newer msysgit versions.  Since I'm no longer with the company that had a proxy implemented, I'm not able to test out the setup.

*Update*: Scott Chacon recently [posted](http://progit.org/2010/03/04/smart-http.html) about Smart HTTP transport which is basically the ability to perform git commands over HTTP with authentication, but unfortunately, it only works for git clients version 1.6.6 and above.  [msysgit (Git for Windows)](http://code.google.com/p/msysgit/) is currently on version 1.6.5.1.  I will write a post about it once there's a new version of msysgit.  [Git for OS X](http://code.google.com/p/git-osx-installer/) has the supported clients.

[Jeff Tchang](http://returnbooleantrue.blogspot.com/2009/06/using-github-through-draconian-proxies.html) does a pretty nice job explaining how to use git commands via SSH through the corporate proxy on Unix and Windows.  I'm just going to fill in a few missing gaps.  This is a straight guide for making git work on Windows in your corporate environment.

## What You'll Need

* [msysgit](http://code.google.com/p/msysgit/) (I used 1.6.5.1)
* [connect.c binary](http://www.taiyo.co.jp/%7Egotoh/ssh/connect.exe)

## Installation

Install msysgit with OpenSSH option (should be the last one in red).  Follow these [instructions](http://help.github.com/msysgit-key-setup/) (if you have not already) to generate SSH keys.  If you prefer the command prompt like me (help guide uses cygwin aka Git Bash), then follow these simple instructions:

* Open command prompt (cmd.exe)
* Navigate to %HOME%

<pre><code class="no-highlight">
C:\Windows\System32>cd %HOME%

C:\Users\skim>
</code></pre>

* Enter the following:

<pre><code class="no-highlight">
C:\Users\skim>ssh-keygen -t rsa -C "email@address.com"
</code></pre>

where "email@address.com" is your own email address linked to your GitHub account.

* It will now prompt you for a filename, simply press *Enter*
* Now it will ask you for a passphrase.  Now for the sake of simplicity, I'm not going to enter a passphrase, but you should read this [help guide](http://help.github.com/working-with-key-passphrases/) on why you should use a passphrase.
* Re-enter your passphrase (in my case, I left it empty and pressed *Enter*)

<pre><code class="no-highlight">
C:\Users\skim>ssh-keygen -t rsa -C "email@address.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/skim/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/skim/.ssh/id_rsa.
Your public key has been saved in /c/Users/skim/.ssh/id_rsa.pub.
The key fingerprint is:
e8:ae:60:8f:38:c2:98:1d:6d:84:60:8c:9e:dd:47:81 email@address.com
</code></pre>

At this point, it created two important files under %HOME%\\.ssh : *id\_rsa* and *id\_rsa.pub*

<pre><code class="no-highlight">
C:\Users\skim>dir .ssh
 Volume in drive C has no label.
 Volume Serial Number is 4C30-D3E0

 Directory of C:\Users\skim\.ssh

02/07/2010  07:43 PM    &lt;DIR>          .
02/07/2010  07:43 PM    &lt;DIR>          ..
02/06/2010  11:10 PM             1,675 id_rsa
02/06/2010  11:10 PM               399 id_rsa.pub
02/07/2010  07:43 PM               407 known_hosts
               3 File(s)          2,481 bytes
               2 Dir(s)  11,605,680,128 bytes free
</code></pre>

The content of *id\_rsa.pub* will be used to add a public key to your [SSH public keys](https://github.com/account#ssh_bucket) list on your GitHub account.  This will allow you to talk to your GitHub repositories from the very computer you are using now.  Make sure you have done this.

The other file, *id\_rsa* will be used to allow access to your GitHub repositories via SSH/proxy.

Under %HOME%\\.ssh, create a new file called *config* and add the following:

<pre><code class="no-highlight">
ProxyCommand C:/git/bin/connect.exe -H username@proxy.example.com:443 %h %p

Host github.com
User git
Port 22
Hostname github.com
IdentityFile "C:\Users\skim\\.ssh\id_rsa"
TCPKeepAlive yes
IdentitiesOnly yes

Host ssh.github.com
User git
Port 443
Hostname ssh.github.com
IdentityFile "C:\Users\skim\\.ssh\id_rsa"
TCPKeepAlive yes
IdentitiesOnly yes
</code></pre>

Make sure to change the following in the above commands to match your environment:

* Change *C:/git/bin/connect.exe* to where you placed connect.exe.  Make sure it's in PATH environment variable.  It's important the slashes in the path are in fact forward slashes.  This was discussed in Jeff Tchang's [article](http://returnbooleantrue.blogspot.com/2009/06/using-github-through-draconian-proxies.html)
* Change *username@proxy.example.com:443* to your proxy address.  If you don't require username/password just enter the address (e.g., *proxy.example.com:443*).  Make sure the port is the port number of your corporate environment's proxy.  It doesn't have to be 443.
* Change both *C:\Users\skim\\.ssh\id\_rsa* paths to your specific %HOME%\\.ssh\id\_rsa path.  It's important the path contains *id\_rsa* and not *id\_rsa.pub*

## Testing It Out

Assuming \_Your\_Git\_Path\_\bin\ folder is in your PATH environment variable, try running the following command:

<pre><code class="no-highlight">
C:\Users\skim>ssh -F %HOME%\skim\.ssh\config github.com

ERROR: Hi sl4m! You've successfully authenticated, but GitHub does not provide shell access

Connection to github.com closed.
</code></pre>

The error is expected since GitHub does not allow shell access.  What is most important is that you see the "successfully authenticated" message.

Now that you're all set, you can run git commands.  It's slightly different from the git commands you might be used to (if you're familiar with git already).  When you clone a repository, you typically invoke this command:

<pre><code class="no-highlight">
git clone git://github.com/rails/rails.git
</code></pre>

But with SSH, this is how you invoke the same command:

<pre><code class="no-highlight">
git clone ssh://git@github.com:443/rails/rails.git
</code></pre>

Follow the pattern and you should be able to run all git commands via SSH/proxy.

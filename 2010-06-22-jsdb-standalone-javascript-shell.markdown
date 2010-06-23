---
layout: post
title: jsdb - standalone javascript shell
date: 2010-06-22 23:00:00 -05:00
categories:
  -- javascript
---

Earlier this year I was looking for a standalone JavaScript interpreter similar to Ruby's IRB to mess around in JavaScript.  [JSDB](http://www.jsdb.org/), while it stands for JavaScript for databases, can easily be used to test JavaScript code snippets and a shell I would always have opened when working with JavaScript.  For the longest time I thought it was only available for Windows, but I found out yesterday it's platform independent and can be installed and run on Mac and Linux as well.

It uses the TraceMonkey JIT compiler with the SpiderMonkey JavaScript engine from Firefox 3.5.  Since it's derived from a specific JavaScript engine, code that might otherwise work on a different engine (e.g., JScript, v8), might not exactly work on this engine.  There are peculiarities between JavaScript engines that give developers headaches, so let this be a warning.

Have fun!

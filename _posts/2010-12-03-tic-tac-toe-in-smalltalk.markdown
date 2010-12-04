---
layout: post
title: tic tac toe in smalltalk
date: 2010-12-03 18:30:00 -07:00
categories:
  -- smalltalk
  -- tdd
---

During my [apprenticeship](http://skim.cc/), I wrote a Tic Tac Toe program in Smalltalk.  Initially, I planned on using [Pharo](http://pharo-project.org/), one of the major Smalltalk implementations, but found out about [GNU Smalltalk](http://smalltalk.gnu.org/) and its POSIX compatibility, and went with that instead.  Unfortunately (or fortunately?), GNU Smalltalk does not give you the full experience of Smalltalk.  Smalltalk is about the living environment experience, not so much about the language and its syntax.  As you start reading the source code, you'll start notice some similarities with the Smalltalk language and Ruby.  This is because Smalltalk was a major influence to Ruby.  This is one of the main reasons why I chose to write Tic Tac Toe in Smalltalk.

Anyway, before I go off on talking about Smalltalk history, the source code is on my GitHub [repo](https://github.com/sl4m/tic_tac_toe_smalltalk).  Here is a sample of what the code looks like:

{% highlight smalltalk %}
Board subclass: ThreeByThree [
  ThreeByThree class >> new [
    ^super new initialize
  ]

  ThreeByThree class >> create: existingBoard [
    ^self new
          list: existingBoard
  ]

  initialize [
    super initialize.
    winningPatterns := #((1 2 3) (4 5 6) (7 8 9) (1 4 7) (2 5 8) (3 6 9) (1 5 9) (3 5 7)).
    list := Array new: 9 withAll: ' '.
  ]

  findPattern: pattern [
    ((list at: (pattern at: 1)) = 'X' & (list at: (pattern at: 2)) = 'X' & (list at: (pattern at: 3)) = 'X')
      ifTrue: [winner := 'X'].
    ((list at: (pattern at: 1)) = 'O' & (list at: (pattern at: 2)) = 'O' & (list at: (pattern at: 3)) = 'O')
      ifTrue: [winner := 'O'].
  ]
]
{% endhighlight %}

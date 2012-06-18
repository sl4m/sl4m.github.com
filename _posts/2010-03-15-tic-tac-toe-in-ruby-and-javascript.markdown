---
layout: post
title: tic tac toe in ruby and javascript
date: 2010-03-15 23:55:00 -07:00
categories:
  -- ruby
  -- javascript
  -- breakable toys
  -- node.js
  -- pomodoro
  -- tdd
---

*Note:* The latest Ruby source can be found [here](http://github.com/sl4m/tic_tac_toe_ruby) and JavaScript source [here](http://github.com/sl4m/tic_tac_toe_js)

## The Challenge

I was "challenged" by someone last week to build a tic tac toe game that should consist of the following:

* human vs. computer
* some form of user interface (text-based is ok)
* computer can never lose, only draw
* can be written in any programming language

I don't recall the last time I created a game, let alone a game with some form of AI.  So what would any person like me do?  Search on [Google](http://google.com/) of course.  I used keywords like *tic tac toe algorithm*, *tic tac toe AI*, and discovered different ways people solved the problem.

My friend who works for Siemens, told me to use a library he uses at work called [rl-library](http://code.google.com/p/rl-library/).  It is a [Reinforcement Learning](http://en.wikipedia.org/wiki/Reinforcement_learning) library that is capable of 'learning' based on user-defined environments.  I found the framework to be very fascinating but I thought it would make the challenge too easy, so I had to opt out (I did add the [Reinforcement Learning book](http://www.amazon.com/exec/obidos/tg/detail/-/0262193981/ref=ord_cart_shr?_encoding=UTF8&m=ATVPDKIKX0DER&v=glance) to my wish list though).  I told him I wanted to avoid all external libraries and he thought about another way.  For the next several hours, we sat down together via Skype, sharing his screen to show me how to develop a table of all possible moves in memory.  I mean it's only 9! (or 362,880) possibilities which will consume some megabytes of memory.  So by the end of the night, we had some code in Java and I needed to complete the state/action piece for this table to be useful - in other words, after the table has been created, I needed to determine which move was the best move (from the computer standpoint).

I was at a loss.  I think it was easy for my friend since he thinks in RL, but I couldn't figure it out right away.  So off to Google I went again, looking for another solution.  I found several search results and ended up finding what became my solution.  I never looked at Tic Tac Toe beyond the rules and playing the game, so it was interesting to learn about the algorithm that the computer would use.  This [solution](http://webster.cs.ucr.edu/AsmTools/MASM/TicTacToe/ttt3_1.html) basically had a similar table, but not all 362,880 possibilities.  It used pattern sets in regular expressions to look for a winning move or a blocking move.  Storing these patterns in arrays of arrays, yielded the Ruby code below.

## Tic Tac Toe in Ruby

*Update:* This code is old and has been replaced.  See the latest [here](http://github.com/sl4m/tic_tac_toe_ruby).

<pre><code class="ruby">
# move positions
#
#  0 | 1 | 2
# ---+---+---
#  3 | 4 | 5
# ---+---+---
#  6 | 7 | 8

class TicTacToe
  module Patterns
    Winning =
      [[(/ OO....../),0],[(/O..O.. ../),6],
       [(/......OO /),8],[(/.. ..O..O/),2],
       [(/ ..O..O../),0],[(/...... OO/),6],
       [(/..O..O.. /),8],[(/OO ....../),2],
       [(/ ...O...O/),0],[(/..O.O. ../),6],
       [(/O...O... /),8],[(/.. .O.O../),2],
       [(/O O....../),1],[(/O.. ..O../),3],
       [(/......O O/),7],[(/..O.. ..O/),5],
       [(/. ..O..O./),1],[(/... OO.../),3],
       [(/.O..O.. ./),7],[(/...OO .../),5]]
    Blocking =
      [[(/  X . X  /),1],[(/ XX....../),0],[(/X..X.. ../),6],
       [(/......XX /),8],[(/.. ..X..X/),2],[(/ ..X..X../),0],
       [(/...... XX/),6],[(/..X..X.. /),8],[(/XX ....../),2],
       [(/ ...X...X/),0],[(/..X.X. ../),6],[(/X...X... /),8],
       [(/.. .X.X../),2],[(/X X....../),1],[(/X.. ..X../),3],
       [(/......X X/),7],[(/..X.. ..X/),5],[(/. ..X..X./),1],
       [(/... XX.../),3],[(/.X..X.. ./),7],[(/...XX .../),5],
       [(/ X X.. ../),0],[(/ ..X.. X /),6],[(/.. ..X X /),8],
       [(/ X ..X.. /),2],[(/  XX.. ../),0],[(/X.. .. X /),6],
       [(/.. .XX   /),8],[(/X  ..X.. /),2],[(/ X  ..X../),0],
       [(/ ..X..  X/),6],[(/..X..  X /),8],[(/X  ..X.. /),2]]
    Won =
      [[(/OOO....../),:O], [(/...OOO.../),:O],
       [(/......OOO/),:O], [(/O..O..O../),:O],
       [(/.O..O..O./),:O], [(/..O..O..O/),:O],
       [(/O...O...O/),:O], [(/..O.O.O../),:O],
       [(/XXX....../),:X], [(/...XXX.../),:X],
       [(/......XXX/),:X], [(/X..X..X../),:X],
       [(/.X..X..X./),:X], [(/..X..X..X/),:X],
       [(/X...X...X/),:X], [(/..X.X.X../),:X]]
  end

  def initialize
    @board = [].fill(0, 9) { " " }
    @players = { :X => 'X', :O => 'O' }
    @turn = :X
  end

  def play
    winner_flag = false
    9.times do
      if @turn === :X
        display
        player_move
      else
        cpu_move
      end
      winner = someone_win?
      unless winner.nil?
        display
        print "\n#{winner} is the winner!\n"
        winner_flag = true
        break
      end
      @turn = (@turn === :X) ? :O : :X
    end
    if (!winner_flag)
      display
      print "\nGame is a draw.\n"
    end
  end

  private
  def player_move
    print "Enter your move [0-8]: "
    move_pos = gets.chomp.to_i
    unless (0..8) === move_pos
      print "\nInvalid move: #{move_pos}. Please re-enter.\n"
      player_move
      return
    end
    if @board.at(move_pos) != ' '
      print "\nSpace is already occupied. Please re-enter.\n"
      player_move
      return
    end
    move move_pos, 'X'
  end

  def cpu_move
    move_pos = get_winning_pattern_move
    if move_pos.nil?
      move_pos = get_blocking_pattern_move
      if move_pos.nil?
        move_pos = get_first_available_move
      end
    end
    move move_pos, 'O'
  end

  def move(pos, piece)
    @board.delete_at(pos)
    @board.insert(pos, piece)
  end

  def display
    print "\n\n"
    print " #{@board[0]} |"
    print " #{@board[1]} |"
    print " #{@board[2]}"
    print "\n---+---+---\n"
    print " #{@board[3]} |"
    print " #{@board[4]} |"
    print " #{@board[5]}"
    print "\n---+---+---\n"
    print " #{@board[6]} |"
    print " #{@board[7]} |"
    print " #{@board[8]}"
    print "\n\n"
  end

  def someone_win?
    symbol = nil
    array = Patterns::Won.find { |p| p.first =~ @board.join }
    unless array.nil?
      symbol = (array.last === :X) ? 'X' : 'O'
    end
    symbol
  end

  def get_winning_pattern_move
    move_pos = nil
    array = Patterns::Winning.find { |p| p.first =~ @board.join }
    unless array.nil?
      move_pos = array.last
    end
    move_pos
  end

  def get_blocking_pattern_move
    move_pos = nil
    array = Patterns::Blocking.find { |p| p.first =~ @board.join }
    unless array.nil?
      move_pos = array.last
    end
    move_pos
  end

  def get_first_available_move
    if @board.at(4) == ' '
      move_pos = 4
    else
      move_pos = @board.index(' ')
    end
    move_pos
  end
end

if $0 == __FILE__
  print "\n\nYou are X.  Please go first."
  TicTacToe.new.play
end
</code></pre>

I initially began writing the game in JavaScript, but couldn't figure out a way to display the board in the console without having to deal with the browser.  Luckily, I was pointed in the right direction and used [Node.js](http://nodejs.org/) (sadly does not work on Windows).  It handled the stdout/stdin just fine.  I had to fiddle around with stdin by using Node's process object and add a listener for stdin.  It wasn't immediately intuitive how the listener worked, but I was able to finally get it to prompt for user input multiple times to play the game.

## Tic Tac Toe in JavaScript

<pre><code class="javascript">
/**
  move positions

   0 | 1 | 2
  ---+---+---
   3 | 4 | 5
  ---+---+---
   6 | 7 | 8

*/

exports.game = function() {
  var sys = require('sys');
  var WinningPatterns =
    [[(/ OO....../),0],[(/O..O.. ../),6],
     [(/......OO /),8],[(/.. ..O..O/),2],
     [(/ ..O..O../),0],[(/...... OO/),6],
     [(/..O..O.. /),8],[(/OO ....../),2],
     [(/ ...O...O/),0],[(/..O.O. ../),6],
     [(/O...O... /),8],[(/.. .O.O../),2],
     [(/O O....../),1],[(/O.. ..O../),3],
     [(/......O O/),7],[(/..O.. ..O/),5],
     [(/. ..O..O./),1],[(/... OO.../),3],
     [(/.O..O.. ./),7],[(/...OO .../),5]];

  var BlockingPatterns =
    [[(/  X . X  /),1],[(/ XX....../),0],[(/X..X.. ../),6],
     [(/......XX /),8],[(/.. ..X..X/),2],[(/ ..X..X../),0],
     [(/...... XX/),6],[(/..X..X.. /),8],[(/XX ....../),2],
     [(/ ...X...X/),0],[(/..X.X. ../),6],[(/X...X... /),8],
     [(/.. .X.X../),2],[(/X X....../),1],[(/X.. ..X../),3],
     [(/......X X/),7],[(/..X.. ..X/),5],[(/. ..X..X./),1],
     [(/... XX.../),3],[(/.X..X.. ./),7],[(/...XX .../),5],
     [(/ X X.. ../),0],[(/ ..X.. X /),6],[(/.. ..X X /),8],
     [(/ X ..X.. /),2],[(/  XX.. ../),0],[(/X.. .. X /),6],
     [(/.. .XX   /),8],[(/X  ..X.. /),2],[(/ X  ..X../),0],
     [(/ ..X..  X/),6],[(/..X..  X /),8],[(/X  ..X.. /),2]];

  var WonPatterns =
    [[(/OOO....../),'O'], [(/...OOO.../),'O'],
     [(/......OOO/),'O'], [(/O..O..O../),'O'],
     [(/.O..O..O./),'O'], [(/..O..O..O/),'O'],
     [(/O...O...O/),'O'], [(/..O.O.O../),'O'],
     [(/XXX....../),'X'], [(/...XXX.../),'X'],
     [(/......XXX/),'X'], [(/X..X..X../),'X'],
     [(/.X..X..X./),'X'], [(/..X..X..X/),'X'],
     [(/X...X...X/),'X'], [(/..X.X.X../),'X']];
  var board = [];
  for (var i=0; i&lt;9; i+=1) {
    board.push(' ');
  }
  var X = 'X',
      O = 'O';
  var players = [X, O],
      turn = X;

  var moveCPU = function() {
    var movePos;
    movePos = getWinningPatternMove();
    if (movePos === -1) {
      movePos = getBlockingPatternMove();
      if (movePos === -1) {
        movePos = getFirstAvailableMove();
      }
    }
    move(movePos, O);
  };
  var move = function(pos, piece) {
    if (piece !== turn) { return false; }
    pos = Number(pos);
    if (pos >= 0 &amp;&amp; pos &lt;= 8 &amp;&amp;
      !isNaN(pos) &amp;&amp; board[pos] === ' ') {
      board.splice(pos, 1, piece);
      turn = (piece === X) ? O : X;
      return true;
    }
    return false;
  };
  var getDisplay = function() {
    var display = 
      '\n\n' +
      ' ' + board[0] + ' |' +
      ' ' + board[1] + ' |' +
      ' ' + board[2] +
      '\n---+---+---\n' +
      ' ' + board[3] + ' |' +
      ' ' + board[4] + ' |' +
      ' ' + board[5] +
      '\n---+---+---\n' +
      ' ' + board[6] + ' |' +
      ' ' + board[7] + ' |' +
      ' ' + board[8] +
      '\n\n';
    return display;
  };
  var display = function() {
    sys.print(getDisplay());
  };
  var isBoardFilled = function() {
    var movePos = getFirstAvailableMove();
    if (movePos === -1) {
      display();
      sys.print('Game is a draw.\n');
      process.stdio.close();
      return true;
    }
    return false;
  };
  var isGameWinner = function() {
    var flatBoard = board.join(''),
        i, max = WonPatterns.length, array;
    var gameWinner;
    for (i=0; i&lt;max; i+=1) {
      array = flatBoard.match(WonPatterns[i][0]);
      if (array) { gameWinner = WonPatterns[i][1]; }
    }
    if (gameWinner) {
      display();
      sys.print(gameWinner + ' is the winner!\n');
      process.stdio.close();
      return true;
    }
    return false;
  };
  var getWinningPatternMove = function() {
    var flatBoard = board.join(''),
        i, max = WinningPatterns.length, array;
    for (i=0; i&lt;max; i+=1) {
      array = flatBoard.match(WinningPatterns[i][0]);
      if (array) { return WinningPatterns[i][1]; }
    }
    return -1;
  };
  var getBlockingPatternMove = function() {
    var flatBoard = board.join(''),
        i, max = BlockingPatterns.length, array;
    for (i=0; i&lt;max; i+=1) {
      array = flatBoard.match(BlockingPatterns[i][0]);
      if (array) { return BlockingPatterns[i][1]; }
    }
    return -1;
  };
  var getFirstAvailableMove = function() {
    if (board[4] === ' ') {
      return 4;
    }
    return board.indexOf(' ');
  };
  return {
    play: function() {
      display();
      sys.print("Enter your move [0-8]: ");
      process.stdio.addListener('data', function(response) {
        if (move(response, X)) {
          if (!isGameWinner() &amp;&amp; !isBoardFilled()) {
              moveCPU();
              if (!isGameWinner()) {
                display();
                sys.print("Enter your move [0-8]: ");
              }
          }
        } else {
          sys.print("\nI'm sorry Dave, I'm afraid I can't do that.\n");
          sys.print("Enter your move [0-8]: ");
        }
      });
      process.stdio.open();
    },
    getBoardDisplay : function() {
      return getDisplay();
    },
    getTurn: function() {
      return turn;
    },
    getBoard: function() {
      return board.slice(0);
    },
    getCpuGamePiece: function() {
      return O;
    },
    getHumanGamePiece: function() {
      return X;
    },
    movePlayer: function(pos) {
      return move(pos, X);
    }
  };
};
</code></pre>

Both implementations are a bit [smelly](http://en.wikipedia.org/wiki/Code_smell).  With the time I had, I tried what I could to make it less smelly.  I'm also new to Ruby so I kind of have an excuse ;-).  The JavaScript code is pretty much a straight port from the Ruby code, so it'll include some of the smells.  One thing I was asked to do was to create tests along with the JavaScript code\*.  When thinking about the tests, it made me think how testable the JavaScript TicTacToe object really was.  I ended up adding more public methods to the object to allow the tests to access certain properties of the object.  Had I approached the problem test first, I think it would've been easier to design the code.

\* Of course, I was doing this all wrong from the beginning.  After implementing the code, I then thought about tests which is against [TDD](http://en.wikipedia.org/wiki/Test-driven_development).  One of these days, I'll need to practice TDD and make it a habit.

## Tic Tac Toe Tests in JavaScript

<pre><code class="javascript">
(function() {
  var ticTacToe = require('./tic_tac_toe_node');
  var sys = require('sys');
  var assert = require('assert');

  var runTest = function(description, test) {
    sys.puts('  Testing... ' + description);
    test();
  };

  var runTestSuite = function() {
    sys.puts('\n[Test Suite starting...]\n');

    runTest('board has nine squares', function() {
      assert.strictEqual(ticTacToe.game().getBoard().length,
        9, 'board does not have nine squares');
    });
    runTest('cpu player is O', function() {
      assert.strictEqual(ticTacToe.game().getCpuGamePiece(),
        'O', 'cpu player is not O');
    });
    runTest('human player is X', function() {
      assert.strictEqual(ticTacToe.game().getHumanGamePiece(),
        'X', 'human player is not X');
    });
    runTest('human player starts first', function() {
      assert.strictEqual(ticTacToe.game().getTurn(), 'X',
        'human player is not going first');
    });
    runTest('move human player piece within range [0-8]', function() {
      assert.strictEqual(ticTacToe.game().movePlayer(4), true,
        'could not move piece to position [4]');
    });
    runTest('move human player piece outside of range (lower bound)',
      function() {
        assert.strictEqual(ticTacToe.game().movePlayer(-1), false,
          'was allowed to move piece to position [-1]');
      });
    runTest('move human player piece outside of range (upper bound)',
      function() {
        assert.strictEqual(ticTacToe.game().movePlayer(9), false,
          'was allowed to move piece to position [9]');
      });
    runTest('move human player piece on same square', function() {
      var game = ticTacToe.game();
      game.movePlayer(0);
      assert.strictEqual(game.movePlayer(0), false,
        'was allowed to move piece to same square twice');
    });
    sys.puts('\n[Test Suite finished!]\n');
  };

  var runGame = function() {
    ticTacToe.game().play();
  };

  // run tests
  runTestSuite();

  // run the game
  runGame();

})();
</code></pre>

If you're thinking why the JavaScript version of Tic Tac Toe isn't running on Node.js, it's because it needs to be 'required' from another file due to the [CommonJS module system](http://nodejs.org/api.html#_modules).  Just save the JavaScript TicTacToe code as *tic\_tac\_toe\_node.js* and JavaScript test code as *tic\_tac\_toe\_node\_test.js* in the same folder, and run the test code:

<pre><code class="bash">
node tic_tac_toe_node_test.js
</code></pre>

It should run the small test suite and execute the game.  It's very simple, so it will end after a game.  You'll have to run the test again to run multiple games.

## Final Thoughts

It's amazing how much I've learned from this challenge.  A few things to take away from this experience:

* Practice, practice, practice!
* Build more breakable toys

More often than not, I read material, but never apply it right away or practice them.  I need to be more hands-on and take on more challenges to apply what I've learned.  Another experiment I tried was using the [pomodoro technique](http://www.pomodorotechnique.com/) using [Chromodoro](https://chrome.google.com/extensions/detail/edhkjecdcakijjmlelnjjiohjmlaikhb).  By creating time-boxed sessions, I was able to focus on a single task at a time.  It really helped and I will start using it for other needs.

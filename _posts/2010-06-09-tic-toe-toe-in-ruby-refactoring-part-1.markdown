---
layout: post
title: tic tac toe in ruby - refactoring part 1 
date: 2010-06-09 16:00:00 -05:00
categories:
  -- ruby
  -- breakable toys
  -- tdd
---

One of the many refactoring techniques I can apply to my smelly Tic Tac Toe code is [Extract Class](http://en.wikipedia.org/wiki/Extract_class).  One of the 8th Lighters pointed me to Martin Fowler's book ["Refactoring"](http://www.amazon.com/Refactoring-Improving-Design-Existing-Technology/dp/0201485672/) which lists a good number of refactoring techniques.  In short, Extract Class refactoring is used to extract methods from an overweight class that has lost its purpose to a new class.  I used this refactoring to extract methods that had reference to the board.  While it's not a significant change, it helped me understand how to use RSpec and drive the code via TDD.  Here's the code and tests.

{% highlight ruby %}
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
  end
 
  class Board
    module Pattern
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
    
    attr_reader :board
    attr_reader :winner

    def initialize
      @board = [].fill(0, 9) { " " }
    end

    def occupied?(space)
      if valid_move?(space)
        return (@board.at(space) == " ") ? false : true
      end
      false
    end

    def valid_move?(space)
      (0..8) === space
    end

    def move(space, piece)
      if valid_move?(space)
        @board.delete_at(space)
        @board.insert(space, piece)
      end
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
      array = Pattern::Won.find { |p| p.first =~ @board.join }
      if array
        @winner = (array.last === :X) ? 'X' : 'O'
        return true
      end
      false
    end
  end

  def initialize
    @board = Board.new
    @players = { :X => 'X', :O => 'O' }
    @turn = :X
  end
  
  def play
    winner_flag = false
    9.times do
      if @turn === :X
        @board.display
        player_move
      else
        cpu_move
      end				
      if @board.someone_win?
        @board.display
        print "\n#{@board.winner} is the winner!\n"
        winner_flag = true
        break
      end
      @turn = (@turn === :X) ? :O : :X
    end
    if (!winner_flag)
      @board.display
      print "\nGame is a draw.\n"
    end	
  end
  
  private
  def player_move
    print "Enter your move [0-8]: "
    move_pos = gets.chomp.to_i
    if !@board.valid_move?(move_pos)
      print "\nInvalid move: #{move_pos}. Please re-enter.\n"
      player_move
      return
    end
    if @board.occupied?(move_pos)
      print "\nSpace is already occupied. Please re-enter.\n"
      player_move
      return
    end
    @board.move(move_pos, 'X')
  end
  
  def cpu_move
    move_pos = get_winning_pattern_move
    if move_pos.nil?
      move_pos = get_blocking_pattern_move
      if move_pos.nil?
        move_pos = get_first_available_move
      end
    end
    @board.move(move_pos, 'O')
  end
  
  def get_winning_pattern_move
    move_pos = nil
    array = Patterns::Winning.find { |p| p.first =~ @board.board.join }
    unless array.nil?
      move_pos = array.last
    end
    move_pos
  end
  
  def get_blocking_pattern_move
    move_pos = nil
    array = Patterns::Blocking.find { |p| p.first =~ @board.board.join }
    unless array.nil?
      move_pos = array.last
    end
    move_pos
  end
  
  def get_first_available_move
    if !@board.occupied?(4) 
      move_pos = 4
    else
      move_pos = @board.board.index(' ')
    end
    move_pos
  end
end

if $0 == __FILE__
  print "\n\nYou are X.  Please go first."
  TicTacToe.new.play
end
{% endhighlight %}

{% highlight ruby %}
# tictactoe_spec.rb
require 'tictactoe'

describe TicTacToe::Board, "#valid_move?" do
  it "returns true if move is 0-8" do
    board = TicTacToe::Board.new
    (0..8).each do
      |s| board.valid_move?(s).should == true
    end
  end

  it "returns false if move is not 0-8" do
    board = TicTacToe::Board.new
    board.valid_move?(9).should == false
  end
end

describe TicTacToe::Board, "#move" do
  it "occupies space if move is made within valid range" do
    board = TicTacToe::Board.new
    board.move(0, 'X')
    board.occupied?(0).should == true
  end

  it "does not occupy space if move is made not within range" do
    board = TicTacToe::Board.new
    board.move(9, 'X')
    board.occupied?(9).should == false
  end
end

describe TicTacToe::Board, "#someone_win?" do
  it "returns true if it finds match from pattern set" do
    board = TicTacToe::Board.new
    board.move(0, 'X')
    board.move(1, 'X')
    board.move(2, 'X')
    board.someone_win?.should == true
  end
end
{% endhighlight %}

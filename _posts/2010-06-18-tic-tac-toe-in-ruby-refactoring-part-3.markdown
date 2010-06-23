---
layout: post
title: tic tac toe in ruby - refactoring part 3 
date: 2010-06-18 23:00:00 -05:00
categories:
  -- ruby
  -- breakable toys
  -- tdd
---

The MinMaxPlayer is complete.  I had trouble with the minmax algorithm and did not include tree depth as part of the logic.  Now it seems to be working and I'm completely stoked.  Here is the MinMaxPlayer class and its RSpec tests.

{% highlight ruby %}
require 'player'
require 'board'
require 'ui'

class MinMaxPlayer < Player

  def initialize(piece)
    super(piece)
  end

  def get_opponent(piece)
    return piece === 'O' ? 'X' : 'O'
  end

  def evaluate_score(board, piece)
    opponent = get_opponent(piece)
    case board.winner
    when piece
      return 1
    when opponent
      return -1
    else
      return 0
    end
  end

  def get_best_move(board, piece, depth)
    if board.game_over?
      return evaluate_score(board, piece)
    else
      best_score = -999
      opponent = get_opponent(piece)
      empty_squares = board.get_empty_squares
      empty_squares.each do |s|
        temp_board = Board.new(board.to_a)
        temp_board.move(s, piece)
        score = -get_best_move(temp_board, opponent, depth + 1)
        if score > best_score
          best_score = score
          @best_move = s if depth == 1
        end
      end
      return best_score
    end
  end

  def make_move
    @ui.display_message("Player '#{@piece}' makes a move\n")
    if @board.get_empty_squares.size == @board.size
      return rand(@board.size)
    end
    get_best_move(@board, @piece, 1)
    return @best_move
  end
end
{% endhighlight %}

MinMaxPlayer RSpec tests.

{% highlight ruby %}
require File.expand_path(File.dirname(__FILE__)) + "/spec_helper"
require 'min_max_player.rb'
require 'board'
require 'ui'
require 'stringio'

describe MinMaxPlayer do
  before(:each) do
    @x = 'X'
    @o = 'O'
    @b = ' '
    @ui = UI.new(StringIO.new, StringIO.new)
    @min_max = MinMaxPlayer.new(@x)
    @min_max.ui = @ui
    @board = Board.new
    @min_max.board = @board
  end

  it "should inherit from Player" do
    MinMaxPlayer.ancestors.include?(Player).should == true
  end

  it "should return correct opponent" do
    @min_max.get_opponent(@o).should == @x
    @min_max.get_opponent(@x).should == @o
  end

  it "should return 1 if Max is winner" do
    @board.move(0, @x)
    @board.move(1, @x)
    @board.move(2, @x)
    @min_max.evaluate_score(@board, @x).should == 1
  end

  it "should return -1 if Min is winner" do
    @board.move(0, @o)
    @board.move(1, @o)
    @board.move(2, @o)
    @min_max.evaluate_score(@board, @x).should == -1
  end

  it "should return -1 if Max is winner (from Min POV)" do
    @board.move(0, @x)
    @board.move(1, @x)
    @board.move(2, @x)
    @min_max.evaluate_score(@board, @o).should == -1
  end

  it "should return 1 if Min is winner (from Min POV)" do
    @board.move(0, @o)
    @board.move(1, @o)
    @board.move(2, @o)
    @min_max.evaluate_score(@board, @o).should == 1
  end

  it "should return 0 if no one is winner" do
    board = Board.new([@x, @x, @o, @o, @x, @x, @x, @o, @o])
    @min_max.evaluate_score(board, @x).should == 0
  end

  it "should make winning move" do
    @board.move(0, @x)
    @board.move(1, @o)
    @board.move(2, @x)
    @board.move(3, @o)
    @board.move(4, @x)
    @board.move(5, @o)
    @board.move(6, @o)

    @min_max.make_move.should == 8
  end

  it "should make winning move 2" do
    @board.move(0, @x)
    @board.move(1, @o)
    @board.move(2, @x)
    @board.move(3, @o)

    @min_max.make_move.should == 4
  end

  it "should make winning move 3" do
    @board.move(0, @o)
    @board.move(1, @x)
    @board.move(3, @o)
    @board.move(4, @o)
    @board.move(6, @x)
    @board.move(7, @x)

    @min_max.make_move.should == 8
  end

  it "should make blocking move, scenario 1" do
    @board.move(0, @x)
    @board.move(3, @o)
    @board.move(4, @o)
    @min_max.make_move.should == 5
  end

  it "should make winning move, scenario 3" do
    @board.move(0, @x)
    @board.move(1, @o)
    @board.move(2, @x)
    @board.move(3, @o)
    @board.move(5, @o)
    @board.move(7, @x)

    @min_max.make_move.should == 4
  end

  it "should not make secret losing move" do
    @board.move(0, @x)
    @board.move(4, @o)
    @board.move(8, @o)

    @min_max.make_move.should == 2
  end
end
{% endhighlight %}

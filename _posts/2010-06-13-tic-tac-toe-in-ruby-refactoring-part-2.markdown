---
layout: post
title: tic tac toe in ruby - refactoring part 2
date: 2010-06-13 23:00:00 -05:00
categories:
  -- ruby
  -- breakable toys
  -- tdd
---

Over the weekend, I spent more time refactoring my Tic Tac Toe program.  It now looks completely different from the [original](http://skim.la/2010/03/15/tic-tac-toe-in-ruby-and-javascript) source code.  I attempted to use TDD 100% of the time, and I found myself using it almost all the time.  It took some time getting used to writing tests first, but once I got the hang of it, it felt comfortable.  There's still a lot of work to do, but here is the main file along with some tests.  You can see the rest of the source code on my GitHub [repo](http://github.com/sl4m/tic_tac_toe_ruby). 

<pre><code class="ruby">
# move positions
#
#  0 | 1 | 2
# ---+---+---
#  3 | 4 | 5
# ---+---+---
#  6 | 7 | 8

require 'game'
require 'human_player'
require 'cpu_player'

class TicTacToe
  attr_reader :std_in, :std_out, :player1, :player2, :game
  def initialize(*args)
    @std_in = STDIN
    @std_out = STDOUT
    @std_in = args[0] if args.size > 0
    @std_out = args[1] if args.size > 0
  end

  def ask_for_player(piece)
    player_type = ""
    loop do
      @std_out.print "\nChoose player type for '#{piece}' ('h' for human or 'c' for cpu) "
      player_type = @std_in.gets.chomp
      break if player_type == "h" || player_type == "c"
    end
    if player_type == 'h'
      HumanPlayer.new(piece, @std_in, @std_out)
    else
      CpuPlayer.new(piece)
    end
  end

  def choose_players
    @player1 = ask_for_player('O')
    @player2 = ask_for_player('X')
  end

  def play
    loop do
      choose_players
      @game = Game.new(player1, player2, @std_out)
      @game.play
      play_again = ''
      loop do
        @std_out.print "\nDo you want to play again? ('y' or 'n') "
        play_again = @std_in.gets.chomp
        break if play_again == 'y' || play_again == 'n'
      end
      break if play_again == 'n'
    end
    @std_out.print "\nThanks for playing!"
  end
end

if $0 == __FILE__
  TicTacToe.new.play
end
</code></pre>

The RSpec tests

<pre><code class="ruby">
# tictactoe_spec.rb
require File.expand_path(File.dirname(__FILE__)) + "/spec_helper"
require 'tictactoe'
require 'stringio'
require 'human_player'
require 'cpu_player'

describe TicTacToe do
  before(:each) do
    @std_in = StringIO.new
    @std_out = StringIO.new
    @ttt = TicTacToe.new(@std_in, @std_out)
  end

  it "should be able to create a new instance" do
    lambda { TicTacToe.new }.should_not raise_error
  end

  it "should allow change to stdin" do
    ttt = TicTacToe.new
    ttt.std_in.should == STDIN
    @ttt.std_in.should == @std_in
  end

  it "should allow change to stdout" do
    ttt = TicTacToe.new
    ttt.std_out.should == STDOUT
    @ttt.std_out.should == @std_out
  end

  it "should return player" do
    @ttt.std_in.string = "h"
    @ttt.ask_for_player('O').instance_of?(HumanPlayer)
    @ttt.std_in.string = "c"
    @ttt.ask_for_player('X').instance_of?(CpuPlayer)
  end
end
</code></pre>

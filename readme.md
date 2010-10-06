ActiveModel::StateMachine
=========================

I believe this was pulled from Rails 3 at the last minute, not much documentation exists, but it works very nicely.

Add this line to your bundle:

    gem 'state_machine', :git => 'git://github.com/ivanvanderbyl/state_machine.git', :require => 'active_model/state_machine'
    
Traffic light example
=====================

    class TrafficLight < ActiveRecord::Base
      include ActiveModel::StateMachine

      state_machine do
        state :red
        state :green
        state :yellow

        event :change_color do
          transitions :to => :red,    :from => [:yellow], :on_transition => :catch_runners
          transitions :to => :yellow, :from => [:green]
          transitions :to => :green,  :from => [:red]
        end
      end

      def catch_runners
        puts "That'll be $250."
      end
    end

    light = TrafficLight.new
    light.current_state       #=> :red
    light.change_color!       #=> true
    light.current_state       #=> :green
    light.change_color!       #=> true
    light.current_state       #=> :yellow
    light.change_color!       #=> true
    "That'll be $250."
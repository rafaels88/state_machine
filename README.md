# RMachine

[![Build Status](https://travis-ci.org/rafaels88/rmachine.svg?branch=master)](https://travis-ci.org/rafaels88/rmachine)

## Summary

RMachine is a simple **State Machine** library built for Ruby 2.1+ applications.

Current stable version [v0.1.0](https://github.com/rafaels88/rmachine/tree/v0.1.0).

## Install

Console

`gem install r_machine -v 0.1.0`

or in your Gemfile

`gem 'r_machine', '~> 0.1.0'`


## QuickStart

```ruby
class Entity
  include RMachine

  r_machine do |state|
    state.field = :status
    state.initial = :created

    state.on :event_without_param do |event|
      event.change_to :second_state, from: :created
      event.change_to :third_state, from: :second_state
      event.change_to :fourth_state, from: :third_state, if: :is_this_true?
    end

    state.on :event_with_param do |event|
      event.change_to :second_state, from: :created
      event.change_to :third_state, from: :second_state
      event.change_to :fourth_state, from: :third_state, if: :is_param_correct?
    end
  end

  def event_without_param
    super
  end

  def event_with_param(param_to_verify)
    super
  end

  def is_this_true?
    true
  end

  def is_param_correct?(param_to_verify)
    if param_to_verify == :expected
      true
    else
      false
    end
  end
end

entity = Entity.new
puts entity.status # :created

entity.event_without_param
puts entity.status # :second_state

entity.event_with_param(:any)
puts entity.status # :third_state

entity.event_with_param(:any) # raises StateTransactionNotPermittedError

entity.event_with_param(:expected)
puts entity.status # :fourth_state

entity.status = :created
puts entity.status # :created
```

## Contributing

If you have found any bug, have any idea or anything else which may contribute with this project,
please feel free to create new issues, open/send Pull Requests or make any comment.

## License

RMachine is under MIT license

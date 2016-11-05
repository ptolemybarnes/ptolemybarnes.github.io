---
layout: post
title:  Overloading in Ruby
date:   2016-09-16 18:00
categories: ruby, hacking
---

Following on from dependency annotation in Ruby, I decided to have a go at implementing a way of writing 'overloaded' method definitions. To overload a method or function is to provision it with multiple definitions differing by arity. Here's a naive kind of overloading in Ruby:

```ruby
class Overloaded

  def call(*args)
    case args.count
    when 0
      :no_arguments
    when 1
      :one_argument
    when 2
      :two_arguments
    end
  end

end
```

In this way, `Overloaded` can be called with different numbers of arguments. Using control flow to imitate overloading isn't really overloading, however. Time to stick our hands in the sack of cats that is metaprogramming:

```ruby
class Overloaded

  def self.define(method_name, &block)
    define_method(suffix_with_arity(method_name, block.arity), block)
  end

  def self.suffix_with_arity(method_name, arity)
    (method_name.to_s + '_' + arity.to_s).to_sym
  end

  def method_missing(root_method_name, *args)
    method_name = self.class.suffix_with_arity(root_method_name, args.count)
    if respond_to? method_name
      send(method_name, *args)
    else
      super(method_name, *args)
    end
  end

  define(:call) do
    :no_arguments
  end

  define(:call) do |arg|
    :one_argument
  end

  define(:call) do |first, second|
    :two_arguments
  end

end

# Overloaded.new.call
#  => :no_arguments
# Overloaded.new.call(1)
#  => :one_argument
#
# etc.
```

`#define` gives us an interface to write overloaded methods. There's a problem with this version:

```ruby
Overloaded.new.methods - Object.methods
# => [:method_missing, :call_0, :call_1, :call_2]
```

Overloaded gets cluttered in a surprising way with the set of method definitions for a given 'overloaded' method. >_<

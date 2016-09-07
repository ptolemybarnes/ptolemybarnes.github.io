---
layout: post
title:  "Dynamicity"
date:   2016-09-05 18:00
categories: ruby
---

I was recently expressing my wonder/horror to a friend that it was possible to hack dependency injection/annotation into javascript as the developers of Angular did, when I had the thought: can it be done in Ruby?

Turns out, yes, and it's via the Method class.

```ruby
class DependencyInjector < Struct.new(:klass)

  def call
    args = klass.method(:call).parameters.map do |param_array|
      Kernel.const_get(capitalize(param_array[1].to_s)).new
    end
    klass.call(*args)
  end

  def capitalize(word)
    word[0].capitalize + word[1..-1]
  end

end

class Controller
  def self.call(myFactory, myService)
  end
end

class MyFactory; end
class MyService; end

DependencyInjector.new(Controller).call
 #=> Doesn't blow up!
```

This led me to the idea of attempting, in the spirit of learning, to crowbar the features of other programming languages into Ruby.

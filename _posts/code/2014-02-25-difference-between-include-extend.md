---
layout: post
title: Include vs Extend in Ruby
description: The difference between include and extend in Ruby
date: 2014-02-25
category: code
---

*转载至 [RailsTips](http://www.railstips.org/blog/archives/2009/05/15/include-vs-extend-in-ruby/)*

Now that we know the difference between an instance method and a class method, let’s cover the difference between include and extend in regards to modules. Include is for adding methods to an instance of a class and extend is for adding class methods. Let’s take a look at a small example.

```
module Foo
  def foo
    puts 'heyyyyoooo!'
  end
end

class Bar
  include Foo
end

Bar.new.foo # heyyyyoooo!
Bar.foo # Untitled.rb:12:in `<main>': undefined method `foo' for Bar:Class (NoMethodError)

class Baz
  extend Foo
end

Baz.new.foo # Untitled.rb:18:in `<main>': undefined method `foo' for #<Baz:0x007fa0ac834808> (NoMethodError)
Baz.foo # heyyyyoooo!
```
As you can see, include makes the foo method available to an instance of a class and extend makes the foo method available to the class itself.

## Include Example
If you want to see more examples of using include to share methods among models, you can read my article on how I [added simple permissions to an app](http://railstips.org/2009/4/20/how-to-add-simple-permissions-into-your-simple-app-also-thoughtbot-rules). The permissions module in that article is then included in a few models thus sharing the methods in it. That is all I’ll say here, so if you want to see more check out that article.

## Extend Example
I’ve also got a simple example of using extend that I’ve plucked from the Twitter gem. Basically, Twitter supports two methods for authentication—httpauth and oauth. In order to share the maximum amount of code when using these two different authentication methods, I use a lot of delegation. Basically, the Twitter::Base class takes an instance of a “client”. A client is an instance of either Twitter::HTTPAuth or Twitter::OAuth.

Anytime a request is made from the Twitter::Base object, either get or post is called on the client. The Twitter::HTTPAuth client defines the get and post methods, but the Twitter::OAuth client does not. Twitter::OAuth is just a thin wrapper around the OAuth gem and the OAuth gem actually provides get and post methods on the access token, which automatically handles passing the OAuth information around with each request.

The implementation looks something like this (full file on github):

```
module Twitter
  class OAuth
    extend Forwardable
    def_delegators :access_token, :get, :post

    # a bunch of code removed for clarity
  end
end
```
Rather than define get and post, I simply delegate the get and post instance methods to the access token, which already has them defined. I do that by extending the Forwardable module onto the Twitter::OAuth class and then using the def_delegators class method that it provides. This may not be the most clear example, but it was the first that came to mind so I hope it is understandable.

## A Common Idiom
Even though include is for adding instance methods, a common idiom you’ll see in Ruby is to use include to append both class and instance methods. The reason for this is that include has a self.included hook you can use to modify the class that is including a module and, to my knowledge, extend does not have a hook. It’s highly debatable, but often used so I figured I would mention it. Let’s look at an example.

```
module Foo
  def self.included(base) # this is hook method
    base.extend(ClassMethods)
  end
  
  module ClassMethods
    def bar
      puts 'class method'
    end
  end
  
  def foo
    puts 'instance method'
  end
end

class Baz
  include Foo
end


Baz.bar # class method
Baz.new.foo # instance method
Baz.foo # Untitled.rb:24:in `<main>': undefined method `foo' for Baz:Class (NoMethodError)
Baz.new.bar # Untitled.rb:25:in `<main>': undefined method `bar' for #<Baz:0x007f8de48440d0> (NoMethodError)
```
There are a ton of projects that use this idiom, including Rails, DataMapper, HTTParty, and HappyMapper. For example, when you use HTTParty, you do something like this.

```
class FetchyMcfetcherson
  include HTTParty
end

FetchyMcfetcherson.get('http://foobar.com')
```
When you add the include to your class, HTTParty appends class methods, such as get, post, put, delete, base_uri, default_options and format. I think this idiom is what causes a lot of confusion in the include verse extend understanding. Because you are using include it seems like the HTTParty methods would be added to an instance of the FetchyMcfetcherson class, but they are actually added to the class itself.

## Conclusion
Hope this helps those struggling with include verse extend. Use include for instance methods and extend for class methods. Also, it is sometimes ok to use include to add both instance and class methods. Both are really handy and allow for a great amount of code reuse. They also allow you to avoid deep inheritance, and instead just modularize code and include it where needed, which is much more the ruby way.

---
layout: post
disqus: true
title: "What does 'class << self' in Ruby?"
date: 2018-07-06 18:30:50 -0300
categories: ruby
---

It is used to define class methods within Ruby. It's as simple as that, but let's see an example.

If you want to define a method for a class in Ruby, you can do it in the following way:

```
class Format
 def self.to_html(message)
  # return html code with my message
 end

 def self.to_markdown(message)
  # return markdown code with my message
 end

 def self.to_xml(message)
  # return xml code with my message
 end
end

msg = "Hello word"

puts Format.to_html(msg)
puts Format.to_markdown(msg)
puts Format.to_xml(msg)
```

Here we can see a class that helps to give a differents formats to a message, we also see that an instance of the class is not used to call those methods. Only the methods are called by the class.

But you could also do it in the following way:

```
class Format
  class << self
   def to_html(message)
    # return html code with my message
   end

   def to_markdown(message)
    # return markdown code with my message
   end

   def to_xml(message)
    # return xml code with my message
   end
  end
end
```

And this is the case where we see our `class << self`, simple not?

_Note: the second version allows us to define private methods for a class while in the first one it is not possible._


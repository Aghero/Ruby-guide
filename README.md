### What is Ruby programming language?

Ruby is an interpreted, general-purpose programming language. Everything is an object in Ruby

#### What does it mean that everything is an object?

Lets take simple integer as example in our case. In our beloved Java we could have either an instance of **int** or
an **Integer**
class. The first one is a primitive type, second one would have to be an instance of an Integer class. Ruby takes a
simpler approach.

```ruby
3.times { puts "This is convenient isn't it?" }
```

#### What is the class of the 3 then?

We can quite easily check by calling `.class` method on the instance of class representing the value 3.

```ruby
3.class
3.class.superclass.superclass.superclass
```

P.S. Fixnum class is deprecated since Ruby 2.4, treat it as an Integer.

### Ruby Data Types

#### Not so constant Constants.

A constant is a variable whose name starts with an upper case letter. Constants may not be reassigned from within
instance methods, but can otherwise be changed at will (For example from class methods).

To declare a constant simply begin the name of a variable with a Capital letter.

```ruby
Hard_coding_is_meh = "Hardcoded value as a constant"
Hard_coding_is_meh = "Is it truly a constant?"
```

Source: https://www.ruby-lang.org/en/documentation/faq/4/

## Strings & Symbols

Strings are usually storing sequences of characters. In Ruby they are created everytime they are used, hence why they
should not be used as a hash keys for example

Symbol objects represent names and some strings inside the Ruby interpreter. The same Symbol object will be created for
a given name or string for the duration of a program's execution.

```ruby
str0 = "Sample String"
str1 = "Sample String"

str0.object_id == str1.object_id

def sample_method
  :sample_symbol
end

def another_method
  :sample_symbol
end

sample_method.object_id == another_method.object_id
```

Note that Ruby methods return last value executed within the method. Keyword **return** is redundant in these cases.

Source: https://ruby-doc.org/core-2.5.0/Symbol.html

### Nil

#### Nil is a Ruby's equivalent of a null known from Java world

It represents absence of any value. What would this expression result in then? Exception?

```ruby
nil.class
nil.nil?
```

### True, False and lack of Boolean

There is no boolean class or object per se in Ruby. Instead we have both False and True classes. They serve the same
purpose, storing their respected values. It is worth noting that only nil and false objects are equivalent of false when
it comes to boolean value in Ruby.

`!` operator converts object to the boolean NOT value, hence calling it twice `!!` results in the appropriate boolean
value without negation

What values after converting to boolean values would these objects have?

```ruby
!!nil
!!true
!!false
!![]
!!{}
!!1
!!0
!!""
!!:symbol
```

### Arrays

An Array is an ordered, integer-indexed collection of objects, called elements.
**Any** object may be an Array element.

```ruby
array = Array.new(5) { |i| "Hio_#{i.to_s}" }
array[0]
array[5]
array[5] = {fifth: "How is this even legal?"}
array[-2]
array.take(2)
array << "another_element"
array.insert(2, "third element")
array.pop
array
```

Another useful part of Array class is arrays intersection

```ruby
arr1 = ["1", {hash: "within_array"}, "3"]
arr2 = ["3", :symbol, 5]
common = arr1 & arr2
```

Source: https://ruby-doc.org/core-3.1.2/Array.html

### Hashes

A Hash maps each of its unique keys to a specific value. A Hash has certain similarities to an Array, but:

1. An Array index is always an Integer.
2. A Hash key can be (almost) any object.

```ruby
hash = {ruby: "convenient", java: "robust", c: "reliable", 1 => "what?"}
hash[:ruby]
hash["java"]
```

We can conveniently go through the whole hash by using enumerators

```ruby
hash = {ruby: "convenient", java: "robust", c: "reliable", 1 => "what?"}
hash.each { |key, value| print "#{key}:#{value} \n" }
```

Hashes have default value which is returned in case of absence of a key. Default value is `nil`, however this can be
changed by calling the `.default` method or during the initialisation.

Ruby allows us to transform hash data on the fly, without affecting the hash itself.

```ruby
hash = {ruby: "convenient", java: "robust", c: "reliable", 1 => "what?"}
hash.map { |key, _| key.to_s.concat("_modified") }
hash
```

Modifying hash is straightforward.

```ruby
hash = {ruby: "convenient", java: "robust", c: "reliable", 1 => "what?"}
hash[:new_key] = "this_is_easy"
hash.delete_if { |key, value| key.eql?(1) }
```

Source: https://ruby-doc.org/core-3.1.2/Hash.html

### Exceptions

Ruby provides deals with exceptions in not surprising way by implementing `begin`, `rescue`, `ensure`
mechanism

```ruby

begin
  raise StandardError
rescue NoMethodError, SyntaxError => ex
  puts "An error has occurred"
ensure
  puts "This will always happen"
end
```

Beginning and end of an method acts in the same way as begin block hence this is an equivalent of beforehand presented
block

```ruby

def vague_begin_within_method
  raise NoMethodError
rescue NoMethodError, SyntaxError => ex
  puts "An error has occurred"
ensure
  puts "This will always happen"
end
```

### Classes, modules and mixins

**Classes** in OOP languages act in similar way to legos, you treat them as small pieces that allow building compound
structures (applications). They hold attributes and methods related to their purpose.

**A Module** is a collection of methods and constants. The methods in a module may be instance methods or module
methods. Instance methods appear as methods in a class when the module is included, module methods do not.

Modules have another, wonderful use. At a stroke, they pretty much eliminate the need for multiple inheritance,
providing a facility called **a Mixin**.

```ruby
# Module
module CivilisedBehaviours
  # Module method
  def self.greeting
    puts "Good morning!"
  end

  # Instance method
  def introduction
    puts "Hello. I am an instance of: #{self.class}"
  end
end

#Class
class Person
  # After this line our class becomes a mixin (mixed in one, class and module)
  include CivilisedBehaviours

  attr_accessor :name, :surname, :age, :employment_status

  # Ruby's way of constructing new object of a Person class
  def initialize(name, surname, age, employment_status = "unemployed")
    self.name = name
    self.surname = surname
    self.age = age
    self.employment_status = employment_status
  end

  # Class method
  def self.is_human?
    true
  end

  # Instance method
  def is_smart?
    false
  end

  def detailed_introduction
    puts "Hello. I am #{name} #{surname}. I am #{age} years old. And I am currently #{employment_status}"
  end
end
```

```ruby

module Descriptor
  def describe
    puts "#{self.class}, #{self.to_s}"
  end
end

class FancyConsumer
  include Descriptor
end

class BoringConsumer
  include Descriptor
end
```

### Inheritance

Class and modules, mixins all in one

```ruby

class BasicBase
  def base_method
    print "Based Hello!"
  end

  private

  def secret_method
    print "My precious secrets!"
  end
end

module Descriptor
  def describe
    print "#{self.class}, #{self.to_s} within Module"
  end

  private

  def talked_privately
    print "Gossiping"
  end
end

class WeirdConsumer < BasicBase
  include Descriptor
  include FancyDescriptor
end
```

Sources: https://ruby-doc.org/core-2.5.0/Module.html, https://ruby-doc.com/docs/ProgrammingRuby/html/tut_modules.html
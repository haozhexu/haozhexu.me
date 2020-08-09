---
title: "Ruby Notes"
date: 2020-07-19T18:00:00+11:00
draft: false
ads: true
categories:
  - Notes
tags:
  - ruby
---
# Ruby Notes

This is the notes I've taken while reading the book "The Well-Grounded Rubyist", I tried to make it useful as a reference but do not expect this to be a well-written tutorial for beginners.

[Basics](#basics)  
[Objects, methods and local variables](#objects-methods-and-local-variables)  
[Classes](#classes)
[Modules](#modules)
[Default object, scope and visibility](#default-object-scope-and-visibility)

## Basics

### Basic operations:

- arithmetic: `+, -, *, /`, mixing integers and floating-point numbers would proce floating-point result.
- assignment: `x = 3`, `text = "Hi"`
- comparison: `x == y`
- conversion: `x = "100".to_i`, `x = s.to_i`

### Basic I/O:

- `print "Hi"`: prints the text and leaves the cursor at the end
- `puts "Hi"`: prints the text and adds a new line if there isn't one
- `x = "Hi"`, `p x`: outputs an _inspect_ string which may contain extra information
- `gets`, `text = gets`: get a line of keyboard input

### Special Objects

```ruby
true
false
nil
self
```

For conditional expression, both `false` and `nil` is evaluated as `false`.

Comments are ignored by the interpreter:

```ruby
# a line of comment
x = 3 # inline comment
```

### Convention

Note: I've seen coding conventions that are different from below, so either agree to a convention from the beginning, or use whatever convention that's already been adopted in existing code.

- _Local Variables_: start with a lowercase letter or an underscore, consists of letters, underscores and/or digits. `x`, `first_name`
- _Instance Variables_: stores information within individual objects, always start with a single at-sign (@) and consist of the same character set as local variable. `@age`, `@level`
- _Class Variables_: store information per class, follow the same rule as instance variable except that they start with two at-signs. `@@total_score`
- _Global Variables_: start with a dollar sign. `$FILE_PATH`
- _Constants_: begin with an uppercase letter. e.g. `FirstName`, `LOAD_PATH`
- _Method Names_: they follow the same rule as local variables, and this is on purpose so that the concept of methods is the same as variables as expression that provide a value

### Objects

If you know what OO is, the idea of _Objects_ in Ruby is very straightforward:

- _almost_ everything in Ruby are objects
- every object is capable of receiving a certain set of _messages_
- each _message_ correspond to a _method_

e.g.

```ruby
x = "100".to_i
y = "120".to_i(9)
puts "Hi" # bareword-style invocation
```

## Installation, Package, Version

Sometimes it's useful to have multiple Ruby version installed, to check current configuration, load a library package called `rbconfig`:

```ruby
irb --simple-prompt -r rbconfig
>> RbConfig::CONFIG[term]
```

Directories lookup for _term_:

| Term        | Directory contents                               |
|-------------|--------------------------------------------------|
| rubylibdir  | Ruby standard library                            |
| bindir      | Ruby command-line tools                          |
| archdir     | Architecture-specific extensions and libraries   |
| sitedir     | Your own or third-party extensions and libraries |
| vendordir   | Third-party extensions and libraries (in Ruby)   |
| sitelibdir  | Your own Ruby language extensions (in Ruby)      |
| sitearchdir | Your own Ruby language extensions (in C)         |

Check default load paths:

`ruby -e 'puts $:'`

`-e` means inline script to the interpreter

- `load` is a command for loading library. `load "../extras.rb"`
- `require` a _feature_ only load it once, unlike `load`. `require "../extras"`
- `require_relative` loads features by searching relative to the running directory

### Ruby tools and applications

- ruby - interpreter
- irb - interactive Ruby interpreter
- rdoc and ri - Ruby documentation tools
- rake - Ruby make, a task-management utility
- gem - a Ruby library and application package-management utility
- erb - a templating system

#### Interpreter command-line arguments

| Switch      | Description                                                                                                                             |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| `-c`        | checks the syntax of a program file without executing it, usually used in conjunction with `-w`                                         |
| `-w`        | gives warning messages during program execution                                                                                         |
| `-e`        | executes the code provided in quotation marks on the command line, for multiple lines use literal line breaks (ie. Enter) or semicolons |
| `-l`        | line mode: prints a newline after every line of output                                                                                  |
| `rname`     | requires the named feature, `ruby -rscanf`                                                                                              |
| `-v`        | shows Ruby version and executes the program in verbose mode                                                                             |
| `--version` | shows Ruby version information                                                                                                          |
| `-h`        | shows information about all command-line switches for the interpreter                                                                   |

#### rake

`rake` provides functionalities similar to `make`, it reads and executes tasks defined in file, `Rakefile`.

```Ruby
namespace :project do
  desc "Clean files temporarily generated during building"
  task :clean do
    # cleaning
  end
end
```

- perform the task: `rake project:clean`
- list all defined tasks: `rake --tasks`
- namespace can have any depth: `rake project:build:prod`

#### gem

`gem install library`

- `gem` downloads gem files as needed from rubygems.org
- files in `.gem` format saved in cache subdirectory of gems directory
- you can also install a gem on your hard disk: `gem install /home/me/mygems/something.gem`
- providing a gem without full filename would look for it in current directory and local cache, also searches remotely for dependencies unless providing `-l` command-line flag
- to uninstall a gem, use `gem uninstall gemname`
- once a gem is installed, you can `require` it

## Objects, methods and local variables

### Generic Object

- Generic object doesn't represent or model anything specific
- Created by `Object.new`
- There's also `BasicObject` which is more basic

```ruby
robot = Object.new

def robot.talk
  puts "I am a robot"
end
```

- now we can send _message_ to the object: `robot.talk`

#### Methods that take arguments

```ruby
def robot.c2f(c)
  c * 9.0 / 5 + 32
end
```

- now we can ask `robot` to convert celsius to fahrenheit: `puts robot.c2f(100)`
- parentheses are optional when there's a direct correspondence between parameters and method
- define: `def robot.c2f c`
- call: `robot.c2f 100`
- keyword `return` is optional but sometimes necessary, e.g. `return a, b, c` would wrap the values in an array, equivalent to `[a, b, c]` without `return`

#### Another Example: Product

```ruby
product = Object.new

def product.name
  "A Robot"
end

def product.full_description
  "A Robot that can Talk"
end

def product.is_complete?
  false
end
```

- a method of a boolean value ends with a question mark so it can be used as a condition like asking a question
- everything in Ruby has a Boolean value, and sometimes it's not what you might expect

#### Built-in Methods of an Object

To see a list of innate methods of object:

`p Object.new.methods.sort`

- `object_id` can be used to uniquely identify objects, it's like a reference to object
- `respond_to?` can be used to check if object can respond to a message. `robot.respond_to?("talk")`
- `send` can be used to send message to an object. `robot.send("talk")`
- `__send__` is an alternative way to call `send`, it avoid conflict with dev defined `send`
- there's also the `public_send` which cannot call the object's private methods

### Method Arguments

- providing wrong numbers of arguments yields: `ArgumentError`
- optional argument starts with a star: `def multi_args(*x)`
- arguments can have default values `def default_args(a,b,c=1)`
- Ruby tries to assign values to as many variables as possible
- you can't put the argument sponge to the left of any default-valued arguments

```ruby
def mixed_args(a,b,*c,d)
  puts "Arguments:"
  p a,b,c,d
end

mixed_args(1,2,3,4,5)
# Arguments:
# 1
# 2
# [3, 4]
# 5

mixed_args(1,2,3)
# 1
# 2
# []
# 3

def args_unleashed(a,b=1,*c,d,e)
  p a,b,c,d,e
end

args_unleashed(1,2,3,4,5)
# 1
# 2
# [3]
# 4
# 5

args_unleashed(1,2,3)
# 1
# 1
# []
# 2
# 3
```

```ruby
def broken_args(x,*y,z=1)
end
```

- once given x its argument and sponged up all the remaining arguments in y, nothing can be left for z
- if z gets the right-hand argument, leaving the rest for y, it makes no sense to describe z as "optional" or "default-valued"

### Local variables and assignment

- local variables have limited _scope_
- most variables in Ruby hold _references_ to objects
- immediate values: integers, `true`, `false` and `nil`
- you can't do this: `x = 1; x++`
- `dup` method duplicates an object, preventing original object from being changed inside methods
- `freeze` method freezes an object, prevents it from being changed at all
- there is no way to unfreeze
- `clone` is similar to `dup`, except the `clone` of a frozen object will also be frozen, but not the case with `dup`

## Classes

- classes usually exist for the purpose of being _instantiated_
- classes are objects, the `new` method is a _constructor_ whose purpose is to create and return a new instance of the class
- classes are defined with `class` keyword, named with _constants_
- _instance methods_ will be shared by all instances
- define the same method multiple times would _override_
- reopen a class by `class [ClassName]` with additions or changes

```ruby
class Robot
  def name
    "A Robot"
  end
  def name
    "B Robot"
  end
end

robot = Robot.new
puts robot.name

# reopen class
class Robot
  def age
    return 10
  end
end
```

### Instance Variables

- you can have constructor `initialize` with arguments
- Ruby allows you to define methods that end with an equal sign
- with syntactic sugar, method call is like assignment

```ruby
class Robot
  def initialize(name,age)
    @name = name
    @age = age
  end
  def name
    @name
  end
  def age
    @age
  end
  def get_older
    @age = @age + 1
  end
  def cost=(amount)
    @cost = amount
  end
  def cost
    @cost
  end
end

robot = Robot.new("A Robot", 11)
robot.cost=(100.00)
# syntactic sugar
robot.cost = 100.00
```

- Ruby provides shortcuts for writing getters and setters
- an _attribute_ is a property of an object whose value can be read and/or written through the object
- Ruby offers `attr_reader` and `attr_writer` for defining readable and writable attributes.
- for read-write attributes, use `attr_accessor`

```ruby
class Robot
  attr_reader :name, :age, :cost
  attr_writer :cost
  # same as:
  attr_reader :name, :age
  attr_accessor :cost
end
```

### Inheritance

- _single inheritance_: every Ruby class can have only one superclass
- every class is either a subclass of `Object`, or a sub-subclass of `Object`
- classes are special objects which have the power to create new instances
- class can be created the same we as other objects: `Class.new`
- you can add instance methods to a class when it's created

```ruby
class SmartRobot < Robot
  attr_accessor :ai
end
```

```ruby
c = Class.new do
  def say_hi
    puts "Hi!"
  end
end

c.new.say_hi
```

### Singleton Method, Class Method

```ruby
def Robot.most_expensive(*robots)
  robots.max_by(&:cost)
end
```

- `&:` is often used with the map method

### Constants

- name of constants begins with a capital letter
- constant lookup notation: a double colon (`::`), `Robot::MODELS`

## Modules

- `Class` class is a subclass of the `Module` class, every class object is also a module object
- `Kernel` module contains majority of the methods common to all objects
- modules don't have instances, they get _mixed in_ to classes using `include` or `prepend`
- more than one module can be mixed in, no class can inherit from more than one class

```ruby
module MyModule
  def ruby_version
    system("ruby -v")
  end
end

class MyClass
  include MyModule
end
obj = MyClass.new
obj.ruby_version
```

e.g. a simple FILO stack module

```ruby
module Stacklike
  def stack
    @stack ||= []
  end
  def add(obj)
    stack.push(obj)
  end
  def take
    stack.pop
  end
end
```

```ruby
require_relative "stacklike"
class Stack
  include Stacklike
end
```

- `require` and `load` take strings as their arguments
- `include` takes the name of a module in the form of a constant
- `require` and `load` are locating and loading disk files
- `include`, `extend` and `prepend` perform a program-space, in-memory operation
- a common sequence is to `require` a file then `include` a module defined in the file
- convention: class name is a noun whereas module's name is an adjective, e.g. `Stack` objects are `stacklike`
- by mixin a module into a class, class can define domain language appropriate for the class

### Method Lookup

When an object is sent a message:

- does the object's class have such method?
- does the object's class mixin any modules that have such method?
- does the object's class' superclass define such method?
- does the object's class' superclass mixin any module that define such method?
- if not found, a special method `method_missing` is called
- if method-lookup path includes two or more same-named method, the first one encountered is executed
- if a class mixes in two or more modules with duplicated methods, the one in most recently mixed-in module is used
- mixin a module more than once is ignored, and has no effect in term of the order of method-lookup
- `include` vs `prepend`: method lookup searches the module first if module is `prepend`
- `include` vs `extend`: `extend` makes a module's method available as class methods
- use `super` to jump up to the next-highest definition in the method lookup path

```ruby
module M
  def report
    puts "'report' method in module M"
  end
end
class C
  include M
  def report
    puts "'report' method in class C"
    super
  end
end
```

How `super` handles arguments:

- without argument list, `super` automatically forwards the arguments passed to the method from which `super` is called
- with empty argument list, ie. `super()`, sends no arguments to the higher-up method
- with specific arguments, e.g. `super(a, b, c)`, sends exactly those arguments
- the method `method` returns an instance of the methods, e.g. `obj.method(:method)`
- `super_method` returns an instance of the super method, e.g. `obj.method(:method).super_method`
- `Kernel` module provides an instance method `method_missing(m, *args)`

Module can be used to organize code:

```ruby
module Tools
  class Hammer
  end
end

h = Tools::Hammer.new
```

## Default object, scope and visibility

- `self` at the top level is the default default object, `main`
- `self` inside a class definition is the class object itself
- `self` inside any method is the object to which the message was sent
- `self` inside a class method is the class object
- `self` inside an instance method is the instance of the class

Alternative ways of defining class methods:

```ruby
class C
  def self.x
  end
end

class C
  class << self
    def x
    end
  end
end
```

### Scope and Variables

#### Global

- _global scope_ covers the entire program, global variables are recognizable by initial dollar-sign
- Ruby has a large number of global variables initialized on start
  - `$:` contains the directories that make up the path Ruby searches external file
  - `$0` contains the name of the startup file for the currently running program
  - `$$` contains the process ID
  - `require "English"` has the list of human readable names for global variables

#### Local

- _local scope_ is the basic layer of every Ruby program
- the top level has its own local scope
- every class or module-definition block has its own local scope, same for nested
- every method definition has its own local scope
- every call to a method generates a new local scope with all local variables reset to an undefined state

#### Constants

```ruby
module M
  class C
    class D
      module N
        X = 1
      end
    end
    puts D::N::X # 1
  end
end

## refer to X
M::C::D::N::X
```

- constants have a kind of global visibility, as long as you know the path to a constant through classes and/or modules
- constant lookup is similar to searching a file in a directory
- sometimes it's useful to state constant-lookup at top level to avoid confusion with built-in class or module, e.g. custom `Violin::String` vs native `::String`
- double colon at the beginning means start searching at top level

#### Class variables

- class variables begin with two at-signs, e.g. `@@var`
- class variables are visible to a class and its instances
- class variables are class-hierarchy-scoped
- since Ruby classes are instances of `Class`, instance variables also work on classes

```ruby
class Camera
  @@models = []
  @@cameras = {}
  @@total_count = 0
  attr_reader :model
  def self.total_count
    @total_count ||= 0
  end
  def self.total_count=(n)
    @total_count = n
  end
  def self.add_model(model)
    unless @@models.include?(model)
      @@models << model
      @@cameras[model] = 0
    end
  end
  def initialize(model)
    if @@models.include?(model)
      puts "Creating a new #{model}!"
      @model = model
      @@cameras[model] += 1
      self.class.total_count += 1
    else
      raise "No such model: #{model}."
    end
  end
  def model_count
    @@cameras[self.model]
  end
end

class Leica < Camera
end

Camera.add_model("M9")
Camera.add_model("M10")
Camera.add_model("X-Pro3")
f1 = Camera.new("X-Pro3")
l1 = Leica.new("M9")
l2 = Leica.new("M10")
puts "There are #{Camera.total_count} Cameras!"
puts "There are #{Leica.total_count} Leicas!"
# Leica would have its own total count
# Output:
# Creating a new X-Pro3!
# Creating a new M9!
# Creating a new M10!
# There are 1 Cameras!
# There are 2 Leicas!
```

### Method-access rules

- instance methods defined below `private` are private
- use `public` or `protected` to reverse the effect
- `private` can also take as arguments a list of the methods:
  - `private :method1, :method2`
- `private` method cannot be called with explicit receiver
- private setter can be called with receiver `self`
  - `def manufacture_year=(years)`
- `protected` methods can be called on object `x` as long as the default object (`self`) is an instance of the same class as `x` or an ancestor or descendant class of `x`'s class
- subclasses inherit the method-access rules of their superclasses
- top-level method is stored as a private instance method of `Object` class
- top-level methods must be called in bareword style due to `private` forbidding explicit receiver
- private instance methods of `Object` is available anywhere in the code because `Object` lies in the method-lookup path of every class
- Ruby has a list of built-in private instance methods like `puts` and `print`

## Control Flow

- conditional: execution depends on the truth of an expression
- loop: a single piece of code is executed repeatedly
- iteration: a call to a method is supplemented with a segment of code that the method can call one or more times during its own execution
- exception: error conditions

### Conditional


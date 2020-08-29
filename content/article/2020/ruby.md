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
[Control Flow](#control-flow)  
[Built-in basics](#built-in-basics)  
[Strings, symbols and other scalar objects](#strings-symbols-and-other-scalar-objects)  
[Collection and container objects](#collection-and-container-objects)  
[Enumerable and Enumerator](#enumerable-and-enumerator)  
[File and IO][#file-and-io]  
[Regular Expression][#regular-expression]

## Basics

### Basic operations

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

The `if`, `elsif` and `else` together evaluate to an object, the value is whatever is represented by the code in the successful branch, if the `if` statement doesn't succeed anywhere its value is `nil`.

```ruby
if condition
  # code executed if condition is true
end

# in one line:
if condition then code end

# semicolons as line breaks
if condition; code; end

if condition
  # code executed if condition is true
else
  # code executed if condition is false
end

if condition1
  # code
elsif condition2
  # code
else condition3
  # code
end
```

Negate a codition using not, ! or `unless`:

```ruby
# negate a condition usually have parentheses
if not condition
if !condition

# alternative way for negating a condition
unless condition

unless x > 100
  puts "Small number!"
else
  puts "Big number!"
end
```

Conditional modifiers are usually used for simple condition, without `else` or `elsif` branching.

```ruby
puts "Big numbers!" if x > 100
puts "Pass!" unless x < 50
```

When Ruby parser sees sequence _identifier_, _equal-sign_, and _value_, it allocates space for local variable without assignment of a value:

```ruby
if false
  x = 1
end
# the value of x is nil since parser allocates space for x without a value
```

Assignment in a conditional test evaluates to the value of the assignment, some times it's needed:

```ruby
if name = /ha/.match(name)
  puts "Found a match!"
  puts m
else
  puts "No match"
end
```

#### case

A `case` statement starts with an expression and goes through a list of possible matches, each possible match is contained in a `when` statement consisting of one or more possible matching objects and a segment of code.

```ruby
answer = gets.chomp
case answer
when "yes"
  puts "Good-bye!"
  exit
when "no"
  puts "Good!"
else
  puts "Unknown answer"
end
```

It's possible to have more than one possible match in a single `when`:

```ruby
case answer
when "y", "yes"
  puts "Bye!"
  exit
```

It's also possible to start a `case` statement without test expression, this is an alternative way of `if` statement:

```ruby
case
when camera.model == "Leica"
  puts "You got a Leica!"
when camera.manufactured_year < 2000
  puts "Your camera is old!"
end
```

- every Ruby object has a `case equality` method `===`
- three equal signs, _case subsumption_, _threequal operator_
- outcome of === determins whether a `when` has matched
- each `case` statement evaluates to a single object
- the value of entire `case` statement is the code in matching `when` clause
- if no matching `when` clause the entire statement evaluates to `nil`

```ruby
# equivalent
when "yes"
if "yes" === answer
if "yes".===(answer)

# override ===
class Camera
  def ===(other_camera)
    self.model == other_camera.model
  end
end
```

### Loops

Ruby allows looping repeatedly through code with conditional logic:

- `while` a given condition is true
- `until` a given condition is true
- `unconditionally` break out of a loop

```ruby
loop { code }
loop do
  code
end
```

Terminates the loop:

```ruby
n = 1
loop do
  n += 1
  break if n > 9
end
```

Skip to the next iteration without finishing current iteration:

```ruby
n = 1
loop do
  n += 1
  next unless n == 10
  break
end
```

Conditional looping with `while` and `until`:

```ruby
while condition
  # code executed when condition is true
end
```

`while` can also work with `begin` / `end`:

```ruby
begin
  # code executed
end while condition
```

Like `if` and `unless`, `while` has also a pair `until`:

```ruby
until condition
  # code executed if condition is false
end

begin
  # code executed
end until condition
```

There's also a shorter way to use `while` and `until`:

```ruby
n = n + 1 until n == 10
n = n + 1 while n < 10
```

Looping through a list of values using `for`:

```ruby
scores = [30, 50, 70, 38, 48, 41]
PASS = 50
for score in scores
  if score >= PASS
    puts "Pass!"
  else
    puts "Fail!"
  end
end
```

- loop is an _iterator_ which is a Ruby method that expects a _code block_

### Iterators and code blocks

- an iterator is a Ruby method that expects a _code block_
- inside an iterator the method can _call_ the code block using `yield`
- a code block isn't an argument
- every method has the following syntax:
  - a receiver object or variable (defaulting to `self`)
  - a dot
  - a method name
  - a list of arguments (optional, defaults to `()`)
  - a clode block (optional)

`String#split` for example, splits its receiver on the delimiter argument and returns an array of splitted components, with a block, `split` also yields the split components to the block, one at a time.

```ruby
array = [1, 2, 3]
array.map { |n| n * 10 }
array.map do |n| n * 10 end
```

Implementing different types of loops using `yield`:

```ruby
def my_loop
  while true
    yield
  end
end

def my_simpler_loop
  yield while true
end
```

```ruby
class Integer
  def my_times
    c = 0
    until c == self
      yield c
      c += 1
    end
    self # return the value itself
  end
end

rtn = 3.my_times { |i| puts "I'm on iteration #{i}" }
```

```ruby
class Array
  def my_each
    c = 0
    until c == size
      yield self[c]
      c += 1
    end
    self
  end
end

array = [1, 2, 3, 4, 5]
array.my_each { |e| puts "I got #{e}" }
```

```ruby
class Array
  def my_map
    c = 0
    acc = []
    until c == size
      acc << yield self[c]
      c += 1
    end
    acc
  end
end

names = ["Tom", "Bill"]
names.my_map { |name| name.upcase }
```

A simpler `my_map` using `my_each`:

```ruby
class Array
  def my_map
    acc = []
    my_each { |e| acc << yield e }
    acc
  end
end
```

### Block parameters and variable scopes

- blocks have direct access to variables that already exist
- block parameters are different from non-parameter variables even if they have the same name
- to preserve existing variables, define a block-local variable
  - `array.each do |e;local|`
  - semicolon followed by a variable name indicates the variable is block-local
  - changing such variable won't affect existing one that has the same name
  - block-variables are not considered block parameters and don't get bound to anything when the block is called
  - they are _reserved names_

### Error handling and exceptions

Common exceptions:

- runtime error: raised by `raise`
- NoMethodError: sending a message to an object which cannot resolve the message to a method name, raised by default `method_missing`
- NameError: an identifier cannot be resolved as a variable or method name by interpreter
- IOError: reading a closed stream, writing to a readonly stream and similar operations
- Errno::error: errors relates to file I/O
- TypeError: a method receives an argument it cannot handle
- Argument-Error: using wrong number of arguments

Use `rescue` to catch and handle exception:

```ruby
n = gets.to_i
begin
  result = 100 / n
rescue
  puts "Invalid number for division 100/#{n}"
  exit
end
puts "100/#{n} is #{result}."
```

It's a good practice to specify what exceptions you want to handle, to catch specific exception:

```ruby
rescue ZeroDivisionError
```

The beginning of a method or code block provides an implicit `begin`/`end` context, as a result, using `rescue` inside a method or code block won't need `begin`:

- `rescue` applies to any failing statement preceding it within the block
- to get more fine grained control for `rescue`, explicit `begin`/`end` wrapper is still needed

```ruby
def load_camera_profile
  filename = gets.chomp
  file = File.open(filename)
  yield file
  file.close
  rescue
    puts "Cannot open camera profile #{filename}!"
end
```

- it's necessary to have a `return` inside rescue otherwise the method continues to execute

```ruby
def load_camera_profile
  filename = gets.chomp
  begin
    file = File.open(filename)
  rescue
    puts "Cannot open camera profile #{filename}!"
    return
  end
  yield file
  file.close
end
```

For `NoMethodError` when calling a method on `nil`, save navigation is allowed using `&.` instead of `.`:

```ruby
if collection.cameras&.first&.model == "Leica"
  puts "Your first camera is a Leica!"
end
```

#### binding.irb

- Ruby provides a way to debug by opening an `irb` session from anywhere using `binding.irb`
- put `binding.irb` would cause execution pause at that line with an `irb` session
- the `irb` session has the context of any code executed to this point for inspection

#### Raising exceptions

- exceptions can be raised by `raise exception, message`, e.g. `raise ArgumentError, "invalid input"`
- without exception after `raise`, `RuntimeError` is default
- to assign the exception object to a variable, use `=>` along with `rescue`
  - the object has useful information about the exception, such like `backtrace` and `message`
  - e.g. `rescue ArgumentError => e`
- exception raising is classed based, `raise ZeroDivisionError` instead of `raise ZeroDivisionError.new`
- use `ensure` to execute code before the method finishes
- custom exception can be created by subclassing `Exception` class (or its subclasses)
  - e.g. `class MyException < Exception; end`

Re-raise catched exception:

```ruby
begin
  file = File.open(filename)
rescue => e
  puts "Cannot open file #{filename}"
  # Ruby is smart enough to figure out it's the same exception to raise
  raise
end
```

```ruby
def line_from_file(filename, substring)
  file = File.open(filename)
  begin
    line = file.gets
    raise ArgumentError unless line.include?(substring)
  rescue ArgumentError
    puts "Invalid line!"
    raise
  ensure
    file.close
  end
  return line
end
```

## Built-in basics

### Literal constructors

| Class       | Literal constructor              | e.g.                               |
|-------------|----------------------------------|------------------------------------|
| `String`    | Quotation marks                  | "new string", `new string`         |
| Symbol      | Leading colon                    | `:symbol`, `:"symbol with spaces"` |
| Array       | Square brackets                  | [1, 2, 3, 4, 5]                    |
| Hash        | Curly braces                     | {"Name": "Tom"}                    |
| Range       | Two or three dots                | 0..9, 0...12                       |
| Regexp      | Forward slashes                  | /([a-z]+)/                         |
| Proc lambda | Dash, arrow, parentheses, braces | `->(x, y) { x*y}`                  |

### Syntactic sugar

- Ruby provides syntax sugar for certain operations like method-calling
- the operators can be overridden

```ruby
class Meter
  attr_accessor :exposure
  def initialize(start_exposure=0)
    self.exposure = start_exposure
  end
  def +(e)
    self.exposure += e
  end
  def -(x)
    self.exposure -= e
  end
  def to_s
    exposure.to_s
  end
end

meter = Meter.new(1)
meter += 1
meter -= 2
```

### Overriding unary operators

- unary operators `+` and `-` can be overridden by defining the method `+@` and `-@`
- logical not (!) operator can also be defined

```ruby
class Saturation
  def initialize(s=0)
    @sat = s
  end
  def to_s
    @sat.to_s
  end
  def +@
    @sat + 1
  end
  def -@
    @sat - 1
  end
  def !
    -@sat
  end
end

saturation = Saturation.new
puts +saturation
puts !saturation
puts (not saturation) # not saturation
```

### Bang (!) methods

- Ruby methods canend with an exclamation point (!)
- there's no significance to Ruby internall
- only conventional for marking a method "dangerous"
- e.g. _permanently_ modifies receiver, _mutates_ the state of caller
- advice: only use ! when there's a method of the same name without !

### Built-in and custom `to_*` methods

Ruby offers a number of built-in methods whose name consist of `to_` plus an indicator of a class to which method method converts an object.

- `to_s` to string
- `to_sym` to symbol
- `to_a` to array
- `to_i` to integer
- `to_f` to float

- `puts` calls `to_s` on its arguments
- `puts` on array gives a representation based on calling `to_s` on each of the elements
- `to_s` called on array returns only a single string representation
- every object except `BasicObject` has method `inspect` which prints memory dump of calling object
- `display` takes an argument of a writable output stream, default is `STDOUT`

### Array conversion

- `to_a` provides an array-like representation of objects
- `to_a` is defined on `Array`, not on `Object`
- _bare list_ means several identifiers or literal objects separated by commas
  - `[1, 2, 3, 4, 5]` is an array of the bare list `1, 2, 3, 4, 5`
- _bare list_ can be used for method arguments
  - `def method(arg1, arg2)`
  - `arguments = [1, 2]; method(*arguments)`
- objects can be used as arrays if implementing `to_ary`
- `to_ary` is called where array and only array will work. e.g. array concatenation

### Number conversion

- `to_i` converts the object to an integer
- strings that have no reasonable integer equivalent (e.g. "Hello") converts to 0
- strings that start with digits but also include non-digits, the non-digits part is ignored for `to_i`
- `to_f` converts the objects to floating-point
- methods `Integer` and `Float` exist but are stricter than `to_i` and `to_f`
- objects that respond to `to_str` will have `to_str` representation used as the argument to `String#+`
- `to_str` is also used on arguments to the `<<` (append to string) method

### Boolean

- every expression in Ruby evaluates to an object
- every object has a Boolean value of either `true` or `false`
- `true` and `false` are both objects

Some expressions' values:

- `class MyClass; end` evaluates to `nil`
- `class MyClass; 1; end` evaluates to `1`
- `def m; return false; end` evaluates to `:m`
- `"Hello, world!"` evaluates to `"Hello, world!"`
- `100 > 50` evaluates to `true`
- the only objects that have a Boolean value of false are `false` and `nil`

### The special object nil

- `nil` is the only instance of the class `NilClass`
- the Boolean value of `nil` is false
- `nil` denotes an absence of anything
  - `["one", "two"][3]` evaluates to `nil`
- `nil.to_s` is empty string
- the integer representation of `nil` is 0
- `nil`'s object ID is 8

### Objects comparison

- Ruby's built-in module `Comparable` provides ways to compare objects
- equality tests: `Object` class defines `==`, `eql?` and `equal?`
- redefine `==` for comparison
  - `String` compares if the value of strings are identical
  - `Numeric` (a super class of `Integer` and `Float`) redefines `==` for comparing number values with type conversion
  - `Numeric`'s `eql?` doesn't do type conversion

To make an object `Comparable`:

- mix-in the module `Comparable`
- define a comparison method with the name `<=>` as an instance method of the object
- `<=>` is usually called the _spaceship operator_ or _spaceship method_
- inside the method you define what is _less than_, _equal to_ and _greater than_

```ruby
class Camera
  include Comparable
  attr_accessor :rating
  def <=>(other_camera)
    # the following can also be simplified as:
    # self.rating <=> other_camera.rating
    if self.rating < other_camera.rating
      -1
    elsif self.rating > other_camera.rating
      1
    else
      0
    end
  end
end
```

### Inspecting objects

- `methods` shows what the object, class or module responds to
- `instance_methods` on class shows all the instance methods that the instances of the class has
- `instance_methods(false)` shows without the methods from ancestors

## Strings, symbols and other scalar objects

- string interpolation doesn't work with single-quoted strings
  - `"One plus one is #{1 + 1}"`: "One plus one is 2"
  - `'One plus one is #{1 + 1}'`: "One plus one is #{1 + 1}"
- escape interpolation mechanism in double-quoted string using backslashes
  - `"One plus one is \"\#{1 + 1}\"."`
- alternative quoting mechanisms take the form `%char{text}`
  - `%q{this is a single quoted string}`
  - `%Q{this is a double quoted string}`
  - curly braces are most commonly used but it can be anything
  - opening delimiter has to match closing one
  - `%q-a string-`
  - `%Q/Another string/`
- including unmatched brace of the same type must be escaped
  - `%Q{this () is fine. }`
  - `q(This \( has to be escaped.)`
- _heredoc_ is a string, usually a multiline string defined through `<<` operator
  - `<<EOM` means the text that follows, up to but not including the next occurrence of "EOM"
  - `SQL` is also common for SQL query
  - delimiter must be flush left and be the only thing on the line where it occurs
  - swiching off the flush-left requirement by putting a hyphen before `<<` operator
  - squiggly heredoc `<<~` strips leading whitespace from output
  - by default heredocs are read in as double-quoted strings, thus can include string interpolation
  - to have single-quoted heredoc, enclose the marker with single quote
  - `<<EOM` or any open delimiter doesn't have to be the last thing on its line
    - it's just a placeholder for upcoming heredoc

```ruby
text = << EOM
First line
Second line
EOM

another_text = <<-EOM
The EOM doesn't have to be flush left!
  EOM

single_quoted = <<-`EOM`
this is a
single quoted line
EOM

a = <<EOM.to_i * 10
5
EOM
puts a # output is 50
```

### Basic string manipulation

Use `[]` operator/method to retrieve the nth character in a string, negative numbers index from the end of the string:

```ruby
"Ruby is cool"[5] # "i"
"Ruby is cool"[-1] # "l"
```

A second integer argument `m` gets a substring of `m` characters:

```ruby
"Ruby is cool"[1, 2] # "ub"
```

A single _range_ object can be provided as argument, two dots are inclusive, three dots means exclusive, the second index must always be closer to the end of the string than the first index:

```ruby
'Ruby is cool'[1..3] # "uby"
'Ruby is cool'[1...3] # "ub"
```

Get substring return `nil` if substring isn't found, regular expressions can be used as well for matching.

```ruby
"ruby is cool"["is cool"] # "is cool"
"ruby is cool"["something"] # nil
```

`slice` removes the characters from the string; `slice!` removes permanently

```ruby
string = "ruby is cool"
string.slice!("is ")
string # "ruby cool"
```

Use `[]=` to set part of a string to a new value, integers, ranges, strings and regular expressions all work, if the part isn't found, or nothing matches, `IndexError` is raised:

```ruby
string = "A quick brown fox jumps over a lazy dog."
string["quick"] = "slow"
string[-1] = "!"
string # "A slow brown fox jumps over a lazy dog!"
```

Use `+` for string concatenation, use `<<` to append second string permanently to an existing string, use `#{}` for string interpolation.

```ruby
"Hi" + "there" # "Hi there"
string = "Hi "
string << "there" # "Hi there"
"#{string}there." # "Hi there"
```

### Querying strings

- use `include?` to check if a string includes a substring
- use `start_with?`, `end_with?` to check if string has prefix, suffix
  - `start_with?` also supports regular expression
- use `empty?` to test if a string is empty
- use `size` and `length` to get the size or length of the string
- use `count` to check how many time a given string or letter occurs
- use hyphen-separated range to count how many of a range of letters there are in the string
- `count` with a string checks the number of characters in the string that are in the character set
- to negate the search put a `^` (caret) at the beginning of specification
- `count` can have multiple arguments
- use `index` and `rindex` to get index of substring
- `ord` returns the ordinal code of a character
  - `ord` sent to a string returns ordinal code of the first character
- `chr` returns the character represented by the integer
  - sending `chr` to a number that doesn't represent a character causes a fatal error

```ruby
string = "A quick brown fox jumps over a lazy dog."
string.include?("quick") # true
string.include?("slow") # false
string.start_with?("A") # true
string.end_with?("B") # false
string.start_with?(/[A-Z]/) # true
string.empty? # false
"".empty? # true
string.size # 40
string.count("a") # 2
string.count("g-m") # 6
string.count("aey. ") # 13
string.count("^a-z") # 9 for 8 spaces and one fullstop
string.index("o") # 10
string.rindex("o") # 37
string.ord # 97
"a".ord # 97
97.chr # a
```

### String comparison and transformation

- the `String` class mixes in `Comparable` and defines `<=>`
- `==` is used to test sring content equality
- `String#equal?` checks whether two strings are the same object
- `upcase`, `downcase`, `swapcase`, `capitalize` and `capitalize!`
- `rjust` and `ljust` expand the size of string with space padding
  - second argument provides padding string
  - padding pattern is repeated as many times as it will fit, truncating if necessary
- `center` puts the characters of the string in the center
  - Odd-numbered padding are right-heavy
- use `strip`, `lstrip` and `rstrip` to strip whitespace from either or both sides
- `chop` removes a character unconditionally from end of string
- `chomp` removes a target substring if the string has that suffix
- both `chop` and `chomp` have bang equivalents that change the string in place
- `clear` empties the string
- `replace` replaces string content with specified string in place
- `delete` removes part of the string
- `crypt` performs aDES encryption on the string
- `succ` gets the next-highest string, `"z".succ` is `aa`, `"bzz".succ` is `caa`
- `to_i` converts a string to an integer
  - optional argument specifies the base
  - `"100".to_i(17)` is `289` (base 17 number)
- `oct` converts the string to base 8 number
- `hex` converts the string to base 16 number, `"A".hex` is 10

### Symbols

_Symbols_ are instances of the built-in class `Symbol`, the literal constructor is the leading colon.

- create a symbol programatically by calling `to_sym` or `intern`
- symbol can be converted to a string using `to_s`
- symbols are immutable
- symbols are unique, the same symbol is always the same object

```ruby
"a".to_sym # :a
"hello".intern # :"hello"
:a.to_s # "a"
:xyz.object_id # same as below
:xyz.object_id # same as above
Symbol.all_symbols # return all the symbols

# use grep instead of include?
# include? would first put the symbol to the table
# which makes the result be always true
Symbol.all_symbols.grep(/abc/)
```

Most common uses of symbols are method arguments and hash keys.

- Ruby processes symbols faster
- symbols look good as hash keys

```ruby
atr_accessor :name

# the "send" method also takes symbol as argument
"abc".send(:upcase)

# this also works
some_object.send(method_name.to_sym)

person = { :name => "Tom", :age => 18 }
person[:name] # "Tom"

# simplified hash keys:
person = { name: "Tom", age: 36 }
```

### Numericals

- In Ruby, numbers are objects.
- `Numeric` is the class from which `Float` and `Integer` inherit
- `Numeric` includes useful modules such as `Comparable`

### Times and dates

Three classes exist for times and dates: `Time`, `Date` and `DateTime`.

- `require 'date'` for `Date` and `DateTime`
- `require 'time'` for `Time`

#### Creating date objects

```ruby
Date.today

# send year, month and day
# without parameter, month and day default to 1, year defaults to -4712
Date.new(2020, 8, 14)

# parse a date string
# assumes order of year/month/day
Date.parse("2020/8/14")
```

- Ruby by default expands the century if providing only a one- or two-digit number
- for number equals to or greater than 69, the offset added is 1900
  - `Date.parse("77/8/14")` is 1977-08-14
- for number between 0 and 68 the offset is 2000
  - `Date.parse("03/8/14")` is 2003-08-14
- `Date.parse` tries to understand different formats
  - `"November 2 2013"`
  - `"Aug 14 2020"`
  - `"14 Aug 2020"`
- create Monday-based day-of-week counting `Date` object using `jd` and `commercial` methods
- scan a string against a format spec and generate a `Date` object using `strptime`

#### Creating time objects

```ruby
Time.new # a.k.a now
Time.at(100000000) # number of seconds since the epoch (midnight on 1st Jan, 1970, GMT)
Time.mktime(2020,8,14,6,31,3) # year, month, day, hour, minute and seconds, with reasonable defaults
require 'time'
Time.parse("August 14, 2020, 6:32 am")
```

#### Creating date/time objects

- `DateTime` is a subclass of `Date`
- most common constructors are `new`, `now` and `parse`
- also features the specialized `jd` (Julian date), `commercial` and `strptime` constructors

#### Date/time query methods

The time and date objects have methods that represents themselves.

```ruby
dt = DateTime.now
dt.year
dt.hour
dt.minute
dt.second # DateTime objects have second and sec
t = Time.now
t.month
t.sec # Time objects have only sec
d = Date.today
d.day

t.sunday?
d.saturday?
dt.friday?

d.leap?
dt.leap?
t.dst? # daylight saving time available to Time only
```

#### Date/time formatting methods

```ruby
t = Time.now # 2020-08-14 06:49:19 +1000
t.strftime("%m-%d-%y") # "08-14-20"
Date.today.rfc2822
DateTime.now.httpdate
```

| Specifier | Description                                |
|-----------|--------------------------------------------|
| %Y        | Year (four digits)                         |
| %y        | Year (last two digits)                     |
| %b, %B    | Short month, full month                    |
| %m        | Day of month (left-padded with zeros)      |
| %e        | Day of month (left-padded with blanks)     |
| %a, %A    | Short day name, full day name              |
| %H, %I    | Hour (24-hour clock), hour (12-hour clock) |
| %M        | Minute                                     |
| %S        | Second                                     |
| %c        | "%a %b %d %H:%M:%S %Y"                     |
| %x        | "%m/%d/%y"                                 |

- the `%c` and `%x` specifier may differ from one locale to another
- all of the date/time classes allow for conversion to each other
- in case where target class has more information than source class, missing fields are set to 0

#### Date/time arithmetic

- `Time` objects let you add and subtract seconds, returning a new time object
  - `Time.now - 10`, `Time.now + 30`
- `Date` and date/time objects interpret + and - as day-wise operations
- use `<<` and `>>` for month-wise conversions
- a whole family of `next_`_unit_ and `prev_`_unit_ methods let you move back and forth by days, months or years

```ruby
dt = DateTime.now # 2020-08-14T07:01:01+10:00
puts dt + 10 # add 10 days: 2020-08-24T07:01:01+10:00
puts dt >> 3 # 3 months after: 2020-11-14T07:01:01+10:00
puts dt.next # 2020-08-15T07:01:01+10:00
puts dt.next_year # 2021-08-14T07:01:01+10:00
puts dt.next_month(2) # 2020-10-14T07:01:01+10:00
puts dt.prev_day(10) # 2020-08-04T07:01:01+10:00

d = Date.today # 2020-08-14
next_week = d + 7 # 2020-08-21
d.upto(next_week) { |date| puts "#{date} is a #{date.strftime("%A")}" }
# 2020-08-14 is a Friday
# 2020-08-15 is a Saturday
# 2020-08-16 is a Sunday
# 2020-08-17 is a Monday
# 2020-08-18 is a Tuesday
# 2020-08-19 is a Wednesday
# 2020-08-20 is a Thursday
# 2020-08-21 is a Friday
```

## Collection and container objects

- an _array_ is an ordered collection of objects
- _hashes_ are also ordered collections with objects in pairs
- each pair consists of a _key_ and a _value_

### Array

- array creation using `Array.new`
- `Array` is a method that creates an array with an argument
- if the argument has `to_ary` defined, `Array` calls it to generate an array
- if no `to_ary` defined, `Array` tries to call `to_a`
- otherwise `Array` wraps the argument in an array

```ruby
a = Array.new
Array.new(3) # [nil, nil, nil]
Array.new(2, "ab") # ["ab", "ab"]
Array.new(3) { |i| 10 * (i + 1) } # [10, 20, 30]

Array.new(3, "ef") # uses the same object "ef"
a[0] << "g" # so this would cause each of the three "ef" become "efg"

Array.new(3) { "abc" } # different object "abc"

# literal constructor
a = [1, 2, "three", 4, []]

string = "text"
Array(string) # ["text"]

def string.to_a
  split(//)
end

Array(string) # ["t", "e", "s", "t"]
```

- array can be created also with the `%w` and `%W` constructor
- create array of symbols using `%i` and `%I`
- each of several built-in classes has a class method `try_convert`
  - it takes one argument, looks for a conversion method
  - call the conversion method if exists, or return `nil`

```ruby
# parsing as single quoted string
%w(Tom Jack) # ["Tom", "Jack"]

# parsing as double quoted string
%W{Tom is #{10-2} years old} # ["Tom", "is", "8", "years", "old"]

# space neeed to be escaped
%w(Black\ Smith is a nice person) # ["Black Smith", "is", "a", "nice", "person"]

%i(a b c) # [:a, :b, :c]
```


#### Array manipulation

Simple getter and setter: `[]` and `[]=`

```ruby
a = []
a[0] = "first" # a.[]=(0, "first")
a = [1,2,3,4,5]
a[2] # 3
```

The getter and setter take a second argument as length:

```ruby
a = %w(a quick brown fox jumps over a lazy dog)
a[3,2] # ["fox", "jump"]
a[3, 2] = ["chicken", "run"]
# a is now ["a", "quick", "brown", "chicken", "run", "over", "a", "lazy", "dog"]
a.values_at(3, 8) # ["chicken", "dog"]
```

- the `[]` has a synonym: `slice`
- there's also `slice!` that removes sliced items permanently from the array
- `values_at` takes one or more arguments representing indexes
- elements deep in multi-dimensional array can be retrieved by `dig`
- `dig` takes as arguments the index positions of each nested element
- `unshift` adds an object to the beginning of an array
- `push` adds one or more object to the end of an array
- `<<` adds one object to the end of an array
- `shift` and `pop` are the opposite of `unshift` and `push`
  - also takes argument for number of elements

```ruby
a = [1, 2, 3, 4]
a.unshift(0) # [0,1,2,3,4]
a.push(5) # [0,1,2,3,4,5]
a << 6 # [0,1,2,3,4,5,6]
a.push(7, 8) # [0,1,2,3,4,5,6,7,8]
```

- arrays concatenation using `concat`
- `concat` permanently changes the receiver
- to concatenate and create a new array, use `+`
- `replace` replaces array with another
- `replace` keeps the object but assignment changes the object
- flatten nested array using `flatten`
  - in-place method exists: `flatten!`
- `reverse` reverses an array, also comes with bang version
- `join` returns a string representation of all elements together
- `join` takes an optional argument for the separator
- `uniq` returns an array with duplicates removed

#### Array querying

| Method name                     | Meaning                                          |
|---------------------------------|--------------------------------------------------|
| `a.size(synonym: length, count) | Number of elemnents in the array                 |
| a.empty?                        | whether the array is empty                       |
| a.include?(item)                | true if the array includes item, false otherwise |
| a.count(item)                   | number of occurrences of item in array           |
| a.first(n=1)                    | First `n` elements of array                      |
| a.last(n=1)                     | Last `n` elements of array                       |
| a.sample(n=1)                   | `n` random elements from array                   |

### Hashes

Hash is a colection of objects consisting of key/value pairs, the `=>` operator connects a key on the left with the value corresponding to it on the right.

A hash can be created via:

1. literal constructor (curly braces)
   - `h = {}`
2. `Hash.new` method
   - argument to `Hash.new` is treated as default value for nonexistent hash keys
3. `Hash.[]` method
   - takes a comma separated list of items
   - even number of arguments are treated as alternating keys and values
   - odd number of argument will raise a fatal error
   - also can pass an array of arrays, each subarray has two elements
   - `Hash[ [[1, 2], [3,4]] ] # {1=>2, 3=>4}`
4. top-level method `Hash`
   - empty hash created when called with empty array or `nil`
   - otherwise `to_hash` is called on its single argument

```ruby
weekdays = { "Monday" => "Mon",
             "Tusday" => "Tue",
             "Wednesday" => "Wed",
             "Thursday" => "Thur",
             "Friday" => "Fri",
             "Saturday" => "Sat",
             "Sunday" => "Sun"}
weekdays["Monday"] # "Mon"
```

Adding a key/value pair to a hash:

```ruby
country_names["US"] = "United States"
country_names.[]=("US", "United States")
country_names.store("US", "United States")
```

Retrieving values from a hash:

```ruby
country = country_names["US"] # "United States"
country = country_names.fetch("US")
```

When looking up a nonexistent key:

- `fetch` raises and exception
- `[]` gives either `nil` or the default you specified
- a second argument to `fetch` will be used as default when the key isn't found
- `values_at` also works on hashes, it returns an array of values for specified keys
- `fetch_values` is similar, but raises `KeyError` if the key isn't found
- pass a block to `fetch` or `fetch_values` for default behaviour
- hashes can be nested within other hashes
- similar to `Array`, `dig` also exists for hashes
- supply a code block to `Hash.new` to be executed every time a nonexistent key is referenced

```ruby
h = Hash.new {|hash,key| hash[key] = 0}
```

#### Combining hashes with other hashes

- `update` add the key-value pairs from second hash to the first, overwrite existing keys
- `merge` creates a new hash
- `merge!` is a synonym for `update`

#### Selecting and rejecting elements from a hash

- `select` with a code block to determine if each key-value pair is included in a new array
- `reject` with a code block to determine if each key-value pair should not be included in a new array
- `select` and `reject` have in-place equivalents `select!` and `reject!`
  - return `nil` if the hash doesn't change
  - to make in-place operation return original hash, use `keep_if` and `delete_if`
- `Hash#invert` flips the keys and the values
- be careful since values may not be unique, only one survives with duplicate values
- `Hash#clear` empties the hash
- `compact` works similarly for hashes as it does for array, eliminating keys whose value is `nil`
- `compact!` makes its change in place
- `replace` replaces the content of hash with another hash, in place

```ruby
h = Hash[1,2,3,4,5,6] # {1=>2, 3=>4, 5=>6}
h.select {|k,v| k > 1} # {3=>4, 5=>6}
h.reject {|k,v| k > 1} # {1=>2}
{name: "Tom", address: nil}.compact # {name: "Tom"}
```

#### Hash querying

| method                  | meaning                                 |
|-------------------------|-----------------------------------------|
| `h.has_key?(1)`         | `true` if `h` has the key `1`           |
| `h.include?(1)`         | synonym for `has_key?`                  |
| `h.key?(1)`             | synonym for `has_key?`                  |
| `h.member?(1)`          | synonym for `has_key?`                  |
| `h.has_value?("three")` | `true` if any value in `h` is `"three"` |
| `h.value?("three")`     | synonym for `has_value?`                |
| `h.empty?`              | `true` if `h` has no key/value pairs    |
| `h.size`                | number of key/value pairs in `h`        |

#### Hashes as final method arguments

Ruby allows hash without curly braces when calling a method whose last argument is a hash; If hash is first argument, not only the curly braces are needed, but also the entire arguemnt list need to be in parentheses.

```ruby
def add_to_camera_database(owner, info)
  camera = Camera.new
  camera.owner = owner
  camera.year_made = info[:year_made]
  camera.model = info[:model]
end

add_to_camera_database("Tom",
  year_made: 1991,
  model: "Leica M")
```

#### Named arguments

```ruby
def m(a:, b:)
  p a, b
end

m(a: 1, b: 2) # Ruby matches a and b which are required arguments

def m(a: 1, b: 2) # default values make arguments optional
end
```

If the method's parameter list includes a double-starred name, calling a method using keyword arguments not declared by the method will end up sponging up all unknown keyword arguments into a hash:

```ruby
def m(a: 1, b: 2, **c)
  p a, b, c
end

m(x:1, y:2)
# 1
# 2
# {:x=>1, :y=>2}
```

### Ranges

A _range_ is an object with a start point and an end point.

- _inclusion_: does a given value fall inside the range
- _enumeration_: the range is treated as a traversable collection of individual items

Create a range with `Range.new` or literal constructor.

- ranges with two dots are inclusive
- ranges with three dots are exclusive

```ruby
r = Range.new(1, 100) # 1..100
r = 1..100 # literal range construction 1..100
r = 1...100 # exclusive range
r = Range.new(1, 100, true) # exclusive range 1...100
```

#### Range logic

- ranges can report back their starting and ending points using `begin` and `end`
- a range also knows whether it's an exclusive range by `exclude_end?`
- `cover?` checks whether the argument is within the range using boolean comparison test
- `include?` treats range as a collection of values
- if the range can't be interpreted as a finite collection, `include?` falls back on numerical order and comparison
- backward range is used usually as index

```ruby
r = 1..10
r.begin # 1
r.end # 10
r.exclude_end? # false
r = "a".."z"
r.cover?("a") # true
r.cover?("abc") # true
r.cover?("A") # false
r.cover?([]) # false
r.include?("a") # true
r.include?("abc") # false
r = 1.0..2.0
r.include?(1.5) # true
"This is a sample string"[10..-5] # sample st
```

### Sets

- `Set` is a standard library class, need `require 'set'`
- a _set_ is a unique collection of objects
- internally hash is used, a new key is added to the hash when an element is added to set

Create a new `Set` by providing a collection object, can also provide a code block which every item in the collection object is passed through.

```ruby
cameras = ["Leica M", "X-Pro3", "GX9", "Leica M"]
unique_cameras = Set.new(cameras) # #<Set: {"Leica M", "X-Pro3", "GX9"}>
unique_cameras = Set.new(cameras) {|c| c.upcase} # #<Set: {"LEICA M", "X-PRO3", "GX9"}>
```

- use `<<` to add a single object to a set, nothing happens if adding a duplicated object
- use `delete` to delete an object from a set, nothing happens if the object doesn't exist
- `<<` is also available as `add`
- `add?` returns `nil` if set is unchagned after adding an object
  - this is usually used to determine whether the object already exists

#### Set operations

- `intersection`, aliased as `&`
- `union`, aliased as `+` and `|`
- `difference`, aliased as `-`
- `^` exclusive or, a set consisting of elements that occur in either set but not all
- merging another object into a set depends on how the object iterates over itself
  - merging a hash into a set results in the addition of two-element, key/value arrays to the set

```ruby
camera_brands = Set.new(["Leica", "Fujifilm", "Olympus"])
closed_brands = Set.new(["Olympus"])
optics_brands = Set.new(["Zeiss", "Leica"])
camera_brands - closed_brands # <Set: {"Leica", "Fujifilm"}>
camera_brands + optics_brands # <Set: {"Leica", "Fujifilm", "Olympus", "Zeiss"}>
camera_brands & optics_brands # <Set: {"Leica"}>
```

#### Subset and superset

- `subset?` and `superset?` exist to check subset and superset relation
- `proper_subset?` and `proper_superset?` also make sure superset is bigger and subset is smaller than the original set

## Enumerable and Enumerator

Collection objects in Ruby typically include the `Enumerable` module for common functionalities.

- each class that uses `Enumerable` has to define the instance method `each`

```ruby
class C
  include Enumerable
  def each
    # code
  end
end
```

The job of `each` is to yield items to a supplied code block, one at a time.

```ruby
class Gender
  include Enumerable
  def each
    yield "boy"
    yield "girl"
  end
end

g = Gender.new
g.each do |gender|
  puts "Gender: #{gender}"
end

# output:
# Gender: boy
# Gender: girl
```

`find` works by calling `each`:

```ruby
g = Gender.new
g.find {|gender| g.start_with?('b') }
# output: boy
```

### Enumerable Boolean queries

A number of `Enumerable` methods return true or false depending on whether one or more elements matching certain criteria.

| method     | meaning                                          |
|------------|--------------------------------------------------|
| `include?` | whether the enumerable includes specified object |
| `all?`     | whether ALL elements match query                 |
| `any?`     | whether ANY element matches query                |
| `one?`     | is there one and only one element matching query |
| `none?`    | is there NO element matching query               |

- above methods work also for hashes
- `Hash#each` yields both a key and a value each time
- you can also capture the key/value pair in a single block parameter
  - `hash.each {|pair| …}` with `pair[0]` for key and `pair[1]` for value
- set works naturally with `Enumerable`
- ranges works only if the range can be expressed as a list of discrete elements

### Enumerable searching and selecting

`find` locates the first element in an array for which the code block returns true, failing to find any element that passes code block test would return `nil`.

```ruby
[1,2,3,4,5,6,7,8,9].find {|n| n > 3}
# output: 4
```

As a result, if the array has `nil` as an element, looking for `nil` would always return `nil`. Use `include?` as a work around to check whether the array has `nil` as an element. Alternatively, provide a `Proc` object as an argument to find, the function will be called if `find` fails.

```ruby
failure = lambda { 11 }
over_ten = [1,2,3,4,5,6].find(failure) {|n| n > 10 }
# output: 11
```

`find_all` (the same as `select`) returns a new collection containing all the elements of the original collection that match the criteria in the code block, empty collection is returned if none is found.

`select` generally returns an array, include using `Enumerable` in your own class, but for hashes and sets, `select` returns a hash or set, respectively.

```ruby
a = [1,2,3,4,5,6,7,8,9,10]
a.find_all {|item| item > 5 } # [6, 7, 8, 9, 10]
a.select {|item| item > 100 } # []
```

Arrays, hashes and sets have a bang version `select!` that reduces the collection permanently to only those elements that passed the selection test.

`reject` finds out which elements do not return a true value when yielded to the code block:

```ruby
a.reject {|item| item > 5} # [1, 2, 3, 4, 5]
```

There's also the bang version `reject!`

#### Selecting on threequal matches with grep

`Enumerable#grep` selects from an enumerable object based on case-equality operator `===`.

```ruby
misc = [12, "hi", 10..20, "bye"]
misc.grep(String) # ["hi", "bye"]
misc.grep(10..20) # [12]
```

`enumerable.grep(expression)` is equivalent to:

```ruby
enumerable.select {|element| expression === element }
```

- `grep` can also take a block, it yields each element of its result set to the block

#### Organizing selection results with group_by and partition

- `group_by` on enumerable object takes a block and returns a hash
- the block is executed for each object
- result hash gets a key for each unique block return value
- value for the key is an array of all elements that the block returned that value

```ruby
colors = %w(red orange yellow green blue indigo violet)
colors.group_by {|color| color.size }
# output:
# {3=>["red"], 6=>["orange", "yellow", "indigo", "violet"], 5=>["green"], 4=>["blue"]}
```

- `partition` splits the elements into two arrays based on whether the code block returns true for the element

```ruby
class Camera
  attr_accessor :price
  def initialize(options)
    self.price = options[:price]
  end
  def below(limit)
    (0...limit) === price
  end
end

cameras = 900.step(1200, 50).map {|p| Camera.new(:price => p) }

cameras.partition { |camera| camera.below(1000) }
# output:
# [[#<Camera:0x00007feb871e48b0 @price=900>, #<Camera:0x00007feb871e4838 @price=950>], [#<Camera:0x00007feb871e4798 @price=1000>, #<Camera:0x00007feb871e46f8 @price=1050>, #<Camera:0x00007feb871e4680 @price=1100>, #<Camera:0x00007feb871e4608 @price=1150>, #<Camera:0x00007feb871e4590 @price=1200>]]
```

### Element-wise operations

- `Enumerable#first` returns the first item when iterating over the enumerable
- there is no `Enumerable#last` as not all enumerables have end
- some enumerable classes do have `last` method: `Array` and `Range`
- `take(n)` gets n elements, `drop(n)` gets original collection without the n elements
- use `take_while` and `drop_while` with a code block to constrain the size of "take" by the truth value of the code block
- `min` and `max` returns the minimum and maximum element
  - determined by `<=>` logic
  - can provide a code block to perform non-default criteria
- `minmax` and `minmax_by` returns a pair of values for minimum and maximum
- `min` and `max` for hashes use the keys to determine ordering
  - use `*_by` if you want to use values

```ruby
class Die
  include Enumerable
  def each
    loop do
      yield rand(6) + 1
    end
  end
end

d = Die.new
d.each do |roll|
  if roll == 6
    puts "Start"
  end
end

cameras
# [#<Camera:0x00007feb871e48b0 @price=900>, #<Camera:0x00007feb871e4838 @price=950>, #<Camera:0x00007feb871e4798 @price=1000>, #<Camera:0x00007feb871e46f8 @price=1050>, #<Camera:0x00007feb871e4680 @price=1100>, #<Camera:0x00007feb871e4608 @price=1150>, #<Camera:0x00007feb871e4590 @price=1200>]

cameras.take_while {|c| c.below(1000) }
# [#<Camera:0x00007feb871e48b0 @price=900>, #<Camera:0x00007feb871e4838 @price=950>]

cameras.drop_while {|c| c.below(1000) }
# [#<Camera:0x00007feb871e4798 @price=1000>, #<Camera:0x00007feb871e46f8 @price=1050>, #<Camera:0x00007feb871e4680 @price=1100>, #<Camera:0x00007feb871e4608 @price=1150>, #<Camera:0x00007feb871e4590 @price=1200>]

cameras.min {|a,b| a.price <=> b.price}
# <Camera:0x00007feb871e48b0 @price=900>

cameras.max {|a,b| a.price <=> b.price}
# <Camera:0x00007feb871e4590 @price=1200>
```

- `reverse_each` is `each` but reversely
  - do not use it on infinite iterator
- `Enumerable#each_with_index` yields an extra integer representing the ordinal position of the item
  - `each_with_index` is deprected, use `each.with_index` instead
- provide an argument to `each.with_index` as the first index value, avoiding the need to add one to the index which by default starts from 0
- `each_slice` and `each_cons` walk through a collection a certain number of elements at a time
- `each_slice` handles each element only once
- `each_cons` takes a new grouping at each element with overlapping yielded arrays

```ruby
a = Array(1..10)

a.each_slice(3) {|slice| p slice }
# [1, 2, 3]
# [4, 5, 6]
# [7, 8, 9]
# [10]

a.each_cons(3) {|slice| p slice }
# [1, 2, 3]
# [2, 3, 4]
# [3, 4, 5]
# [4, 5, 6]
# [5, 6, 7]
# [6, 7, 8]
# [7, 8, 9]
# [8, 9, 10]
```

There's a family of `slice_` methods.

- `slice_before` split a collection at the point at which a given criterion is matched
- `slice_after` complements `slice_before`, split a collection after the pattern or Boolean test is found
- `slice_when` tests two elements at a time

```ruby
(1..10).slice_before { |num| num % 2 == 0 }.to_a
# [[1], [2, 3], [4, 5], [6, 7], [8, 9], [10]]

[1,2,3,3,4,5,6,6,7,8,8,8,9,10].slice_when { |i,j| i == j }.to_a
# [[1, 2, 3], [3, 4, 5, 6], [6, 7, 8], [8], [8, 9, 10]]
```

`Enumerable#cycle` yields all the elements in the object again and again in a loop, providing an integer argument asks the loop to run that many times.

#### Enumerable reduction with inject

`inject` works by initializing an accumulator oject and iterating through a collection, performing a calculation on each iteration and resetting the acumulator for purposes of the next iteration, to the result of that calculation.

- without an argument to inject it uses the first element in the enumerable object as the initial value

```ruby
[1,2,3,4].inject(0) {|acc,n| acc + n } # 10
[1,2,3,4].inject(:+)
```

#### Map

- `map` (also callable as `collect`) always returns an array of the same size as the original enumerable
- the elements of returned array consist of the accumulated result of calling the code block on each element in the original object
- `each` exists purely for the side effects from the execution of the block
- `map` on the other hand maintains an accumulator array of the results from the block
- `map` has a in-place `map!`, defined in `Array` and `Set`

```ruby
cameras = %w(Leica Fujifilm Olympus)
cameras.map {|camera| camera.upcase} # ["LEICA", "FUJIFILM", "OLYMPUS"]

# use a symbol argument as a block
cameras.map(&:upcase)
```

Be careful with block evaluation, `map` only cares about return value of `puts` which is always `nil`:

```ruby
array = [1,2,3,4,5]
result = array.map {|n| puts n * 100 } # [nil, nil, nil, nil, nil]
```

#### Strings as enumerables

- `String` has `each_byte`, `each_character` and `each_codepoint`
- `String` has `each_line` that splits the string at the end of each occurrence of global variable `$/`
- `$/` is linebreak by default, can be changed
- drop the `each_` and pluralize the method name give array of all data instead of enumerator

#### Sorting enumerables

In order to make a class instance sortable:

1. define `<=>` for the class
2. place multiple instances in a container, e.g. array
3. sort the container

In cases where no `<=>` is defined for objects, a block can be supplied on the fly to indicate how they should be sorted.

```ruby
sorted_camera = cameras.sort do |a, b|
  a.price <=> b.price
end
```

- `sort_by` always takes a block that requires a treatment of one item in the collection.
- `clamp` takes two argument, returns 1st argument if receiver is less, returns 2nd argument is receiver is greater, otherwise returns the receiver itself

```ruby
cameras.sort_by {|c| c.price }
cameras.sort_by(&:price)
```

### Enumeraors

- an `iterator` is a method that yields one or more values to a code block
- an enumerator is an object
- at heart, an enumerator is a simple enumerable object with an `each` and employs the `Enumerable` module
- an enumerator isn't a container object, the `each` logic has to be explicitly specified

```ruby
e = Enumerator.new do |y|
  y << 1
  y << 2
  y << 3
end

e = Enumerator.new do |y|
  (1..3).each { |i| y << i }
end

e.to_a # [1, 2, 3]
e.map {|x| x * 10} # [10, 20, 30]
e.select {|x| x > 1} # [2, 3]
e.take(2) # [1, 2]
```

- `y` is a _yielder_, an instance of `Enumerator::Yielder` passed to your block
- upon `each`, 1, 2 and 3 are yielded, can also be `y.yield(1)`
- every time the iterator method on the enumerator is called, the code block gets executed once
- it's also possible to involve other objects in the code block

```ruby
a = [1, 2, 3, 4, 5]
e = Enumerator.new do |y|
  total = 0
  until a.empty?
    total += a.pop
    y << total
  end
end

e.take(2) # [5, 9], popping and adding last two
a # [1, 2, 3]
e.to_a # [3, 5, 6], each would pop and add to total until empty
```

#### Attaching enumerators to other objects

When the enumerator need to yield something, it gets the necessary value by triggering the next yield from the object to which it is attached via the designated method.

```ruby
cameras = %w(Leica Olympus Canon Nikon)
e = cameras.enum_for(:select)
e.each {|c| c.include?('o')} # ["Canon", "Nikon"]
```

Any further arguments provided to `enum_for` are passed through to the method to which the enumerator is being attached.

```ruby
e = cameras.enum_for(:inject, "Names: ")
e.each {|string, name| string << "#{name}..." }
# "Names: Leica...Olympus...Canon...Nikon..."
```

- the starting string "Names: " is still alive inside the enumerator
- most built-in iterators return an enumerator when they're called without a block
- the main use for this is _chaining_: calling another method imeediately on the enumerator

```ruby
e = cameras.map
e.each {|camera| camera.capitalize}
```

## Regular Expression

- Regular expressio in Ruby are objects of `Regexp` class
- its purposes is to specify character patterns determined to match (or not match) strings
- a number of Ruby built-in methods take regular expressions as regular expressions
  - `scan`, `substitude`, `split`

Examples of patterns:

- the letter `a` followed by a digit
- any uppercase letter followed by at least one lowercase letter
- three digits followed by a hyphen and four digits
- the beginning of a line followed by one or more whitespaces
- a specific character at the end of a string

Regular expression are instances of `Regexp` class, which has literal constructor `//` and `%r{}`:

```ruby
>> //.class
=> Regexp

>> %r{}.class
=> Regexp
```

Any pattern matching operation involves a regexp and a string. The simplest way to find out whether there's a match between a pattern and a string is with the `match` method or its sibling `match?`. Ruby also features a pattern-matching operator `=~`:

```ruby
"A quick brown fox jumps over a lazy dog.".match?(/fox/)
/fox/.match?("A quick brown fox jumps over a lazy dog.")

puts "Match!" if /Hello/ =~ "Hello world!"
puts "Match!" if "Hello world!" =~ /Hello/
```

- `match?` returns a boolean whereas `match` returns a `MatchData` or `nil`
- `=~` returns the numerical index of the character in the string where the match starts

| symbol | meaning |
|-|-|
| `//`, `%r{}` | instance of `Regexp` class |
| `=~` | determines if a match exists |
| `.` | matches any character except `\n` |
| `\` | escape character, treat next character as literal |
| `[]` | surrounds a character class, matches either character between `[` and `]` |
| `^` | 1. negates a character or character class, matches anything _except_ what follows `^` |
| `^` | 2. matches the expression at the start of a line |
| `\d` | matches any digit |
| `\D` | matches anything except a digit |
| `\w` | matches any digit, alphabets or underscore |
| `\W` | matches anything except a digit, alphabets or underscore |
| `\s` | matches any whitespace characters (space, tab, newline) |
| `\S` | matches anything except a white space character |
| `{}` | matches a character or character class a specific number of times |
| `$` | matches the expression at the end of a line |
| `+` | matches one or more occurrences of the character or character class |
| `*` | matches zero or more occurrences of the character or character class |

A regexp includes three possible components:

1. literal characters
2. the dot wildcard (.), matches any character except `\n`
3. character class

- any literal character in a regexp matches itself in the string
  - e.g. `/b/` matches any string containing letter `b`
- some non-alphanumeric characters have special meanings and need to be _escaped_ to match
  - e.g. `/\?/`
- using `%r{}` syntax doesn't need to escape `/`

The dot widecard character matches _any character_ at some point in the pattern, except of a newline.

```ruby
>> /.ization/.match?("internationalization")
=> true
```

- a _character class_ is an explicit list of characters placed inside square brackets
- also can insert a _range_ of characters
- _negating_ a character class using caret (^) at the beginning of the class

```ruby
regex = %r{[ps]lot}
regex.match?("slot") # true
regex.match?("alot") # false

/[a-z]/
/[A-Fa-f0-9]/

%r{[^A-Fa-f0-9]} # not hex number
```

### MatchData

Use parentheses to specify _captures_ in regexp construction.

e.g. to extract book name from the following line:

```ruby
line = 'the old man and the sea, Hemingway, Mr., writer'
regex = /([A-Za-z\s]+),[A-Za-z\s]+,[\s]?(Mrs?\.)/
m = regex.match(line)
m[0] # "the old man and the sea, Hemingway, Mr."
```

- the question mark after `s` means _match zero or one `s`_
- the result is a `MatchData` object with access to the submatches
- Ruby populates global variables based on numbers
- `$1` contains the substring matched by the subpattern inside the _first set of parentheses from the left_ in the regexp
- `$2` contains the substring matched by the _second_ subpattern, and so forth

```ruby
line = "My phone number is (456) 193-8123."
regex = /\((\d{3})\)\s+(\d{3})-(\d{4})/
m = regex.match(line) # #<MatchData "(456) 193-8123" 1:"456" 2:"193" 3:"8123">
m[0] # "(456) 193-8123"
m[1] # "456"
m[2] # "193"
m[3] # "8123"

>> /((a)((b)c))/.match("abc")
=> #<MatchData "abc" 1:"abc" 2:"a" 3:"bc" 4:"b">
```

## File and IO

The `IO` class handles all input and output streams either by itself or via its descendant classes, particularly `File`.

- `STDERR`, `STDIN` and `STDOUT` are automatically set when the program starts
- if an `IO` object is open for writing, it responds to `puts`
- whatever `puts` is given will be written to that `IO` object's output stream
- `IO` objects are enumerable
- three global variables exist `$stdin`, `$stdout` and `$stderr`, can be reassigned

```ruby
STDIN.select {|line| line =~ /\A[A-Z]/ }
We're only interested in
lines that begin with
Uppercase letters
=> ["We're only interested in\n", "Uppercase letters\n"]

record = File.open("/tmp/record", "w")
old_stdout = $stdout
$stdout = record
$stderr = $stdout
puts "Z record" # written to /tmp/record
z = 10 / 0 # written to /tmp/record
```

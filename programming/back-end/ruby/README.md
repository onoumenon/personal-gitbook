# Ruby

{% embed url="https://www.learnenough.com/ruby-tutorial/hello_world" %}



{% embed url="https://learnxinyminutes.com/docs/ruby/" %}

```
# comment

=begin
multi-line comment
=end

# In Ruby, (almost) everything is an object
3.class # => Integer
"Hello".class # => String

# even methods:
"Hello".method(:class).class # => Method

# arithmetic operators are similar to JS's
1 + 1 #=> 2
8 - 1 #=> 7
10 * 2 #=> 20
35 / 5 #=>7
2 ** 5 #=> 32
5 % 3 #=> 2

# bitwise operators
3 & 5 #=> 1
3 | 5 #=> 7
3 ^ 5 #=> 6

# arithmetic is just synthatic sugar for calling a method
1.+(3) #=> 4
10.* 5 #=> 50
100.methods.include?(:/) #=> true

# special values are objects
nil # equivalent to null in other languages, falsey
true
false

nil.class #=> NilClass
true.class #=> TrueClass
false.class #=> FalseClass

# equality
1 == 1 #=> true
2 == 1 #=> false

# inequality
1 != 1 #=> false
2 != 1 #=> true

# Apart from false itself, nil is the ONLY other falsey value
!!nil #=> false
!!false #=> false
!!0 #=> true
!!"" #=> true

# comparisons (similar to JS)
1 < 10
10 > 1
2 <= 0
2 >= 0

# combined comparison operator: returns `1` when the first
# argument is greater, `-1` when the second argument is greater
# and `0` otherwise
1 <==> 10 #=> -1
10 <==> 1 #=> 1
1 <==> 1 #=> 0

# logical operators
true && false #=> false
true || false #=> true

# There are alternate versions of the logical operators with much lower
# precedence. These are meant to be used as flow-control constructs to chain
# statements together until one of them returns true or false.

# `do_something_else` only called if `do_something` succeeds.
do_something() and do_something_else()
# `log_error` only called if `do_something` fails.
do_something() or log_error()

# String interpolation
placeholder = 'use string interpolation'
"I can #{placeholder} when using double quoted strings"

# You can combine strings with `+` but not other types
'hello ' + 'world' #=> 'hello world'
'hello ' + 3 #=> TypeError: can't convert Fixnum into String
'hello ' + 3.to_s #=> 'hello 3'
"hello #{3}" #=> 'hello 3'

# or combine strings and operators
'hello ' * 3 #=> "hello hello hello"

# or append to string
'hello' << ' world' #=> 'hello world'

# You can print to the output with a newline at the end
puts "I'm printing"
#=> I'm printing
#=> nil

# or print without newline
print "I'm printing"
#=> "I'm printing" => nil

# Assignment
x = y = 10
y #=> 10
x #=> 10

# By convention, use snake_case
# Use descriptive variable names

# Symbols are immutable, reusable constants represented internally by an
# integer value. They're often used instead of strings to efficiently convey
# specific, meaningful values.

:pending.class #=> Symbol
status = :pending
status == :pending #=> true
status == 'pending' #=> false
status == :approved #=> false

# Strings can be converted into symbols and vice versa
status.to_s #=> 'pending'
'argon'.to_sym #=> :argon

# Arrays
array = [1, 2, 3, 4, 5]

# Arrays can contain different types of items.
[1, 'hello', false]

# Arrays can be indexed from both front and back
array[0]
array.first
array[12]
array[-1]
array.last

# or with a start index and length
array[2, 3] #=> [3, 4, 5]

# or with a range
array[1..3] #=> [2, 3, 4]

# returns a new array with reversed values
[1,2,3].reverse #=> [3,2,1]

# or reverse in place with a bang
array.reverse! #=> array == [5,4,3,2,1]

# [var] access is just syntactic sugar

# You can add to an array
array << 6 #[5,4,3,2,1,6]
[1,2,3].push(4)

array.include?(1) #=> true

# Hashes are Ruby's primary dictionary with key/value pairs
hash = { 'color' => 'green', 'number' =>  5 }
hash.keys #=> ['color', 'number']

hash['color'] #=> 'green'
hash['nothing here'] #=> nil

# If using symbols for keys, you can use alternate syntax:
hash = { :defcon => 3, :action => true }
hash = { defcon: 3, action: true }
hash.keys #=> both returns [:defcon, :action]

# Check existence of keys and values in hash
hash.key?(:defcon) #=> true
hash.value?(3) #=> true

# Tip: Both Arrays and Hashes are Enumerable

# Conditionals
if true
    'if statement'
elseif false
    'else if, optional'
else
    'else, also optional'
end

# If a condition controls invocation of one statement rather than a block of code
# you can use postfix-if notation
warnings = ['Patronimic is missing', 'Address too short']
puts("Warning:\n" + warnings.join("\n")) if !warnings.empty?

#Rephrase using unless
puts("Warning:\n" + warnings.join("\n")) unless warnings.empty?

# Loops
(1..5).each do |counter|
    puts "iteration #{counter}"
end

# The 'do |variable| ...end' construct is called a block.
# Blocks are similar to lambdas, anonymous functions or closures
# in other programming languages. Can be passed around as objects,
# called, or attached as methods.

hash.each do |key, value|
  puts "#{key} is #{value}"
end

# If you need an index, you can use 'each_with_index'
array.each_with_index do |element, index|
  puts "#{element} is number #{index} in the array"
end

counter = 1
while counter <= 5 do
    puts "iteration #{counter}"
    counter += 1
end
#=> iteration 1
#=> iteration 2
#=> iteration 3
#=> iteration 4
#=> iteration 5

# .map
array = [1,2,3,4,5]
doubled = array.map do |element|
  element * 2
end
puts doubled
#=> [2,4,6,8,10]
puts array
#=> [1,2,3,4,5]

# Case construct
grade = 'B'

case grade
when 'A'
    puts 'Nice!'
when 'B'
    puts 'Better luck next time.'
when 'C'
    puts 'You can do better.'
else
    puts 'No grades'
end

# Exception handling
begin
  # Code here that might raise an exception
  raise NoMemoryError, 'You ran out of memory.'
rescue NoMemoryError => exception_variable
  puts 'NoMemoryError was raised', exception_variable
rescue RuntimeError => other_exception_variable
  puts 'RuntimeError was raised now'
else
  puts 'This runs if no exceptions were thrown at all'
ensure
  puts 'This code always runs no matter what'
end

# Methods implicitly return value of last statement
def double(x)
    x * 2
end

double(2) #=> 4

# Parentheses are optional if the interpretation is unambiguous.
double 3 #=> 6

double double 3 #=> 12

def sum(x, y)
    x + y
end

# Method arguments are separated by a comma.
sum 3, 4 #=> 7
sum sum(3, 4), 5 #=> 12

# yield
# All methods have an implicit optional block parameter.
# can be called with the 'yield' keyword.
def surround
    puts '{'
    yield
    puts '}'
end

surround { puts 'hello world' }
#=> {
#=> hello world
#=> }

# Blocks can be converted into a 'proc' object, which wraps the block 
# and allows it to be passed to another method, bound to a different scope,
# or manipulated otherwise. This is most common in method parameter lists,
# where you frequently see a trailing '&block' parameter that will accept 
# the block, if one is given, and convert it to a 'Proc'. The naming here is
# convention; it would work just as well with '&pineapple'.

def guests(&block)
    block.class #=> Proc
    block.call(4)
end

guests { |n| "You have #{n} guests." }
#=> "You have 4 guests."

# You can pass in a list with the splat operator (*), which will be converted into an array.
def guests(*array)
    array.each { |guest| puts guest }
end


# In some cases, you will want to use the splat operator: `*` to prompt destructuring
# of an array into a list.
ranked_competitors = ["John", "Sally", "Dingus", "Moe", "Marcy"]

def best(first, second, third)
  puts "Winners are #{first}, #{second}, and #{third}."
end

best *ranked_competitors.first(3) #=> Winners are John, Sally, and Dingus.

# The splat operator can also be used in parameters.
def best(first, second, third, *others)
  puts "Winners are #{first}, #{second}, and #{third}."
  puts "There were #{others.count} other participants."
end

# By convention, all methods that return booleans end with a question mark.
5.even? #=> false
5.odd? #=> true

# By convention, if a method ends with an '!', it does something destructive.
# Many methods have a '!' version to make a change, and a non '!' version to just return a new changed version.
company_name = "Dunder Mifflin"
company_name.upcase #=> DUNDER MIFFLIN
conpany_name #=> Dunder Mifflin
# mutating company_name
company_name.upcase! #=> DUNDER MIFFLIN
company_name #=> DUNDER MIFFLIN

# Classes
class Human
    
    # A class variable. Shared by all instances of the class.
    @@species = 'H. sapiens'
    
    # Basic initializer
    def initialize(name, age = 0)
        # Assign the arg to the 'name' instance variable for the instance
        @name = name
        @age = age
    end
    
    # Basic setter method
    def name=(name)
        @name = name
    end
    
    # Basic getter method
    def name
        @name
    end
    
    # The above functionality can be encapsulated using the attr_accessor method as follows:
    attr_accessor :name
    
    # Getter/ setter methods can also be created individually like this:
    attr_reader :name
    attr_writer :name
    
    # A class method uses self to distinguish from instance methods.
    # It can only be called on the class, not an instance.
    def self.say(msg)
        puts msg
    end
    
    def species
        @@species
    end
end

jim = Human.new('Jim Halpert')
dwight = Human.new('Dwight K. Schrute')

# You can call the methods of the generated object.
jim.species #=> 'H. sapiens'
jim.name #=> 'Jim Halpert'
jim.name = 'Jim Halpert II' #=> 'Jim Halpert II'
jim.name #=> 'Jim Halpert II'
dwight.species #=> 'H. sapiens'
dwight.name #=> 'Dwight K. Schrute'

# Calling of a class method
Human.say('Hi') #=> 'Hi'

# Variable's scopes are defined by the way we name them.
# Variables that start with $ have global scope.
$var = "I'm a global var"
defined? $var #=> global-variable

# Variables that start with @@ have class scope.
@@var = "I'm a class var"
defined? @@var #=> class variable

# Variables that start with @ have instance scope
@var = "I'm an instance var"
defined? @var #=> instance-variable

# Variables that start with a capital letter are constants.
Var = "I'm a constant"
defined? Var #=> constant

# Class is also an object in ruby. So a class can have instance variables.
# A class variable is shared among the class and all of its descendants.

# Base class
class Human
    @@foo = 0
    
    def self.foo
        @@foo
    end
    
    def self.foo=(value)
        @@foo = value
    end
end

# Derived class
class Worker < Human
end

Human.foo #=> 0
Worker.foo #=> 0

Human.foo = 2
Worker.foo #=> 2

# A class instance variable is not shared by the class's descendants.
class Human
    @bar = 0
    
    def self.bar
        @bar
    end
    
    def self.bar=(value)
        @bar = value
    end
end

class Doctor < Human
end

Human.bar #=> 0
Doctor.bar #=> nil

module ModuleExample
    def foo
        'foo'
    end
end

# Including modules binds their methods to the class instances.
# Extending modules binds their methods to the class iteself.
class Person
    include ModuleExample
end

class Book
    extend ModuleExample
end

Person.foo #=> NoMethodError: undefined method 'foo' for Person:Class
Person.new.foo #=> 'foo'
Book.foo #=> 'foo'
Book.new.foo #=> NoMethodError: undefined method 'foo'

# Callbacks are executed when including and extending a module
module ConcernExample
    def self.included(base)
        base.extend(ClassMethods)
        base.send(:include, InstanceMethods)
    end
    
    module ClassMethods
        def bar
            'bar'
        end
    end
    
    module InstanceMethods
        def qux
            'qux'
        end
    end
end

Class Something
    include ConcernExample
end

Something.bar #=> 'bar'
Something.qux #=> NoMethodError: undefined method 'qux'
Something.new.bar #=> NoMethodError: undefined method 'bar'
Something.new.qux #=> 'qux'



```

{% embed url="https://www.synopsys.com/blogs/software-security/ruby-demystified-and-vs" %}

`and or`: They are much easier to understand if you stop thinking about them as boolean operators and start thinking of them as control flow operators! Allow me to explain.



Let’s revisit the above example with this behavior in mind. Using parentheses to illustrate the order of operations, the following illustrates how `&&` and `and` operate differently.

![](https://www.synopsys.com/blogs/software-security/wp-content/uploads/2020/07/ruby-demystified-and-vs-4.jpg)

The low-precedence `or` works in just the same way, short-circuiting execution of its right-hand side if its left-hand side evaluates to a truth-y value (anything but false or nil). It is most commonly used to throw errors if something goes wrong.

![](https://www.synopsys.com/blogs/software-security/wp-content/uploads/2020/07/ruby-demystified-and-vs-7.jpg)

The above example could have just as easily been written like this, depending on your style.

![](https://www.synopsys.com/blogs/software-security/wp-content/uploads/2020/07/ruby-demystified-and-vs-8.jpg)

I prefer the or version in this case as the code gets run from left to right, the same way it is read.

Watch out, though, as the `and` and `or` keywords still have slightly higher precedence than their more mainstream control-flow cohorts (`if`, `unless`, `while`, and `until`).

![](https://www.synopsys.com/blogs/software-security/wp-content/uploads/2020/07/ruby-demystified-and-vs-9.jpg)

…gets evaluated in this order:

![](https://www.synopsys.com/blogs/software-security/wp-content/uploads/2020/07/ruby-demystified-and-vs-10.jpg)

{% embed url="https://rubyinrails.com/2014/01/03/ruby-difference-between-single-and-double-quotes" %}

\

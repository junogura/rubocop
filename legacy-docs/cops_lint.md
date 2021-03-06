# Lint

## Lint/AmbiguousBlockAssociation

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.48 | -

This cop checks for ambiguous block association with method
when param passed without parentheses.

### Examples

```ruby
# bad
some_method a { |val| puts val }
```
```ruby
# good
# With parentheses, there's no ambiguity.
some_method(a) { |val| puts val }

# good
# Operator methods require no disambiguation
foo == bar { |b| b.baz }

# good
# Lambda arguments require no disambiguation
foo = ->(bar) { bar.baz }
```

### References

* [https://rubystyle.guide#syntax](https://rubystyle.guide#syntax)

## Lint/AmbiguousOperator

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.17 | 0.83

This cop checks for ambiguous operators in the first argument of a
method invocation without parentheses.

### Examples

```ruby
# bad

# The `*` is interpreted as a splat operator but it could possibly be
# a `*` method invocation (i.e. `do_something.*(some_array)`).
do_something *some_array
```
```ruby
# good

# With parentheses, there's no ambiguity.
do_something(*some_array)
```

### References

* [https://rubystyle.guide#method-invocation-parens](https://rubystyle.guide#method-invocation-parens)

## Lint/AmbiguousRegexpLiteral

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.17 | 0.83

This cop checks for ambiguous regexp literals in the first argument of
a method invocation without parentheses.

### Examples

```ruby
# bad

# This is interpreted as a method invocation with a regexp literal,
# but it could possibly be `/` method invocations.
# (i.e. `do_something./(pattern)./(i)`)
do_something /pattern/i
```
```ruby
# good

# With parentheses, there's no ambiguity.
do_something(/pattern/i)
```

## Lint/AssignmentInCondition

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.9 | -

This cop checks for assignments in the conditions of
if/while/until.

`AllowSafeAssignment` option for safe assignment.
By safe assignment we mean putting parentheses around
an assignment to indicate "I know I'm using an assignment
as a condition. It's not a mistake."

### Examples

```ruby
# bad
if some_var = true
  do_something
end

# good
if some_var == true
  do_something
end
```
#### AllowSafeAssignment: true (default)

```ruby
# good
if (some_var = true)
  do_something
end
```
#### AllowSafeAssignment: false

```ruby
# bad
if (some_var = true)
  do_something
end
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
AllowSafeAssignment | `true` | Boolean

### References

* [https://rubystyle.guide#safe-assignment-in-condition](https://rubystyle.guide#safe-assignment-in-condition)

## Lint/BigDecimalNew

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.53 | -

`BigDecimal.new()` is deprecated since BigDecimal 1.3.3.
This cop identifies places where `BigDecimal.new()`
can be replaced by `BigDecimal()`.

### Examples

```ruby
# bad
BigDecimal.new(123.456, 3)

# good
BigDecimal(123.456, 3)
```

## Lint/BooleanSymbol

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | No | Yes (Unsafe) | 0.50 | 0.83

This cop checks for `:true` and `:false` symbols.
In most cases it would be a typo.

### Examples

```ruby
# bad
:true

# good
true
```
```ruby
# bad
:false

# good
false
```

## Lint/CircularArgumentReference

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.33 | -

This cop checks for circular argument references in optional keyword
arguments and optional ordinal arguments.

This cop mirrors a warning produced by MRI since 2.2.

### Examples

```ruby
# bad

def bake(pie: pie)
  pie.heat_up
end
```
```ruby
# good

def bake(pie:)
  pie.refrigerate
end
```
```ruby
# good

def bake(pie: self.pie)
  pie.feed_to(user)
end
```
```ruby
# bad

def cook(dry_ingredients = dry_ingredients)
  dry_ingredients.reduce(&:+)
end
```
```ruby
# good

def cook(dry_ingredients = self.dry_ingredients)
  dry_ingredients.combine
end
```

## Lint/Debugger

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.14 | 0.49

This cop checks for calls to debugger or pry.

### Examples

```ruby
# bad (ok during development)

# using pry
def some_method
  binding.pry
  do_something
end
```
```ruby
# bad (ok during development)

# using byebug
def some_method
  byebug
  do_something
end
```
```ruby
# good

def some_method
  do_something
end
```

## Lint/DeprecatedClassMethods

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.19 | -

This cop checks for uses of the deprecated class method usages.

### Examples

```ruby
# bad

File.exists?(some_path)
Dir.exists?(some_path)
iterator?
```
```ruby
# good

File.exist?(some_path)
Dir.exist?(some_path)
block_given?
```

## Lint/DeprecatedOpenSSLConstant

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Pending | Yes | Yes  | 0.84 | -

Algorithmic constants for `OpenSSL::Cipher` and `OpenSSL::Digest`
deprecated since OpenSSL version 2.2.0. Prefer passing a string
instead.

### Examples

```ruby
# Example for OpenSSL::Cipher instantiation.

# bad
OpenSSL::Cipher::AES.new(128, :GCM)

# good
OpenSSL::Cipher.new('AES-128-GCM')
```
```ruby
# Example for OpenSSL::Digest instantiation.

# bad
OpenSSL::Digest::SHA256.new

# good
OpenSSL::Digest.new('SHA256')
```
```ruby
# Example for ::Digest inherited class methods.

# bad
OpenSSL::Digest::SHA256.digest('foo')

# good
OpenSSL::Digest.digest('SHA256', 'foo')
```

## Lint/DisjunctiveAssignmentInConstructor

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | No | No | 0.62 | -

This cop checks constructors for disjunctive assignments that should
be plain assignments.

So far, this cop is only concerned with disjunctive assignment of
instance variables.

In ruby, an instance variable is nil until a value is assigned, so the
disjunction is unnecessary. A plain assignment has the same effect.

### Examples

```ruby
# bad
def initialize
  @x ||= 1
end

# good
def initialize
  @x = 1
end
```

## Lint/DuplicateCaseCondition

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.45 | -

This cop checks that there are no repeated conditions
used in case 'when' expressions.

### Examples

```ruby
# bad

case x
when 'first'
  do_something
when 'first'
  do_something_else
end
```
```ruby
# good

case x
when 'first'
  do_something
when 'second'
  do_something_else
end
```

## Lint/DuplicateHashKey

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.34 | 0.77

This cop checks for duplicated keys in hash literals.

This cop mirrors a warning in Ruby 2.2.

### Examples

```ruby
# bad

hash = { food: 'apple', food: 'orange' }
```
```ruby
# good

hash = { food: 'apple', other_food: 'orange' }
```

## Lint/DuplicateMethods

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.29 | -

This cop checks for duplicated instance (or singleton) method
definitions.

### Examples

```ruby
# bad

def foo
  1
end

def foo
  2
end
```
```ruby
# bad

def foo
  1
end

alias foo bar
```
```ruby
# good

def foo
  1
end

def bar
  2
end
```
```ruby
# good

def foo
  1
end

alias bar foo
```

## Lint/EachWithObjectArgument

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.31 | -

This cop checks if each_with_object is called with an immutable
argument. Since the argument is the object that the given block shall
make calls on to build something based on the enumerable that
each_with_object iterates over, an immutable argument makes no sense.
It's definitely a bug.

### Examples

```ruby
# bad

sum = numbers.each_with_object(0) { |e, a| a += e }
```
```ruby
# good

num = 0
sum = numbers.each_with_object(num) { |e, a| a += e }
```

## Lint/ElseLayout

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.17 | -

This cop checks for odd else block layout - like
having an expression on the same line as the else keyword,
which is usually a mistake.

### Examples

```ruby
# bad

if something
  # ...
else do_this
  do_that
end
```
```ruby
# good

if something
  # ...
else
  do_this
  do_that
end
```

## Lint/EmptyEnsure

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.10 | 0.48

This cop checks for empty `ensure` blocks

### Examples

```ruby
# bad

def some_method
  do_something
ensure
end
```
```ruby
# bad

begin
  do_something
ensure
end
```
```ruby
# good

def some_method
  do_something
ensure
  do_something_else
end
```
```ruby
# good

begin
  do_something
ensure
  do_something_else
end
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
AutoCorrect | `false` | Boolean

## Lint/EmptyExpression

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.45 | -

This cop checks for the presence of empty expressions.

### Examples

```ruby
# bad

foo = ()
if ()
  bar
end
```
```ruby
# good

foo = (some_expression)
if (some_expression)
  bar
end
```

## Lint/EmptyInterpolation

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.20 | 0.45

This cop checks for empty interpolation.

### Examples

```ruby
# bad

"result is #{}"
```
```ruby
# good

"result is #{some_result}"
```

## Lint/EmptyWhen

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.45 | 0.83

This cop checks for the presence of `when` branches without a body.

### Examples

```ruby
# bad
case foo
when bar
  do_something
when baz
end
```
```ruby
# good
case condition
when foo
  do_something
when bar
  nil
end
```
#### AllowComments: true (default)

```ruby
# good
case condition
when foo
  do_something
when bar
  # noop
end
```
#### AllowComments: false

```ruby
# bad
case condition
when foo
  do_something
when bar
  # do nothing
end
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
AllowComments | `true` | Boolean

## Lint/EnsureReturn

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.9 | 0.83

This cop checks for *return* from an *ensure* block.
Explicit return from an ensure block alters the control flow
as the return will take precedence over any exception being raised,
and the exception will be silently thrown away as if it were rescued.

### Examples

```ruby
# bad

begin
  do_something
ensure
  do_something_else
  return
end
```
```ruby
# good

begin
  do_something
ensure
  do_something_else
end
```

### References

* [https://rubystyle.guide#no-return-ensure](https://rubystyle.guide#no-return-ensure)

## Lint/ErbNewArguments

!!! Note

    Required Ruby version: 2.6

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.56 | -

This cop emulates the following Ruby warnings in Ruby 2.6.

% cat example.rb
ERB.new('hi', nil, '-', '@output_buffer')
% ruby -rerb example.rb
example.rb:1: warning: Passing safe_level with the 2nd argument of
ERB.new is deprecated. Do not use it, and specify other arguments as
keyword arguments.
example.rb:1: warning: Passing trim_mode with the 3rd argument of
ERB.new is deprecated. Use keyword argument like
ERB.new(str, trim_mode:...) instead.
example.rb:1: warning: Passing eoutvar with the 4th argument of ERB.new
is deprecated. Use keyword argument like ERB.new(str, eoutvar: ...)
instead.

Now non-keyword arguments other than first one are softly deprecated
and will be removed when Ruby 2.5 becomes EOL.
`ERB.new` with non-keyword arguments is deprecated since ERB 2.2.0.
Use `:trim_mode` and `:eoutvar` keyword arguments to `ERB.new`.
This cop identifies places where `ERB.new(str, trim_mode, eoutvar)` can
be replaced by `ERB.new(str, :trim_mode: trim_mode, eoutvar: eoutvar)`.

### Examples

```ruby
# Target codes supports Ruby 2.6 and higher only
# bad
ERB.new(str, nil, '-', '@output_buffer')

# good
ERB.new(str, trim_mode: '-', eoutvar: '@output_buffer')

# Target codes supports Ruby 2.5 and lower only
# good
ERB.new(str, nil, '-', '@output_buffer')

# Target codes supports Ruby 2.6, 2.5 and lower
# bad
ERB.new(str, nil, '-', '@output_buffer')

# good
# Ruby standard library style
# https://github.com/ruby/ruby/commit/3406c5d
if ERB.instance_method(:initialize).parameters.assoc(:key) # Ruby 2.6+
  ERB.new(str, trim_mode: '-', eoutvar: '@output_buffer')
else
  ERB.new(str, nil, '-', '@output_buffer')
end

# good
# Use `RUBY_VERSION` style
if RUBY_VERSION >= '2.6'
  ERB.new(str, trim_mode: '-', eoutvar: '@output_buffer')
else
  ERB.new(str, nil, '-', '@output_buffer')
end
```

## Lint/FlipFlop

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.16 | -

This cop looks for uses of flip-flop operator.
flip-flop operator is deprecated since Ruby 2.6.0.

### Examples

```ruby
# bad
(1..20).each do |x|
  puts x if (x == 5) .. (x == 10)
end

# good
(1..20).each do |x|
  puts x if (x >= 5) && (x <= 10)
end
```

### References

* [https://rubystyle.guide#no-flip-flops](https://rubystyle.guide#no-flip-flops)

## Lint/FloatOutOfRange

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.36 | -

This cop identifies Float literals which are, like, really really really
really really really really really big. Too big. No-one needs Floats
that big. If you need a float that big, something is wrong with you.

### Examples

```ruby
# bad

float = 3.0e400
```
```ruby
# good

float = 42.9
```

## Lint/FormatParameterMismatch

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.33 | -

This lint sees if there is a mismatch between the number of
expected fields for format/sprintf/#% and what is actually
passed as arguments.

### Examples

```ruby
# bad

format('A value: %s and another: %i', a_value)
```
```ruby
# good

format('A value: %s and another: %i', a_value, another)
```

## Lint/HeredocMethodCallPosition

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Disabled | Yes | Yes  | 0.68 | -

This cop checks for the ordering of a method call where
the receiver of the call is a HEREDOC.

### Examples

```ruby
# bad

   <<-SQL
     bar
   SQL
   .strip_indent

   <<-SQL
     bar
   SQL
   .strip_indent
   .trim

# good

   <<~SQL
     bar
   SQL

   <<~SQL.trim
     bar
   SQL
```

### References

* [https://rubystyle.guide#heredoc-method-calls](https://rubystyle.guide#heredoc-method-calls)

## Lint/ImplicitStringConcatenation

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.36 | -

This cop checks for implicit string concatenation of string literals
which are on the same line.

### Examples

```ruby
# bad

array = ['Item 1' 'Item 2']
```
```ruby
# good

array = ['Item 1Item 2']
array = ['Item 1' + 'Item 2']
array = [
  'Item 1' \
  'Item 2'
]
```

## Lint/IneffectiveAccessModifier

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.36 | -

This cop checks for `private` or `protected` access modifiers which are
applied to a singleton method. These access modifiers do not make
singleton methods private/protected. `private_class_method` can be
used for that.

### Examples

```ruby
# bad

class C
  private

  def self.method
    puts 'hi'
  end
end
```
```ruby
# good

class C
  def self.method
    puts 'hi'
  end

  private_class_method :method
end
```
```ruby
# good

class C
  class << self
    private

    def method
      puts 'hi'
    end
  end
end
```

## Lint/InheritException

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.41 | -

This cop looks for error classes inheriting from `Exception`
and its standard library subclasses, excluding subclasses of
`StandardError`. It is configurable to suggest using either
`RuntimeError` (default) or `StandardError` instead.

### Examples

#### EnforcedStyle: runtime_error (default)

```ruby
# bad

class C < Exception; end

C = Class.new(Exception)

# good

class C < RuntimeError; end

C = Class.new(RuntimeError)
```
#### EnforcedStyle: standard_error

```ruby
# bad

class C < Exception; end

C = Class.new(Exception)

# good

class C < StandardError; end

C = Class.new(StandardError)
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
EnforcedStyle | `runtime_error` | `runtime_error`, `standard_error`

## Lint/InterpolationCheck

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.50 | -

This cop checks for interpolation in a single quoted string.

### Examples

```ruby
# bad

foo = 'something with #{interpolation} inside'
```
```ruby
# good

foo = "something with #{interpolation} inside"
```

## Lint/LiteralAsCondition

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.51 | -

This cop checks for literals used as the conditions or as
operands in and/or expressions serving as the conditions of
if/while/until.

### Examples

```ruby
# bad
if 20
  do_something
end

# bad
if some_var && true
  do_something
end

# good
if some_var && some_condition
  do_something
end

# good
# When using a boolean value for an infinite loop.
while true
  break if condition
end
```

## Lint/LiteralInInterpolation

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.19 | 0.32

This cop checks for interpolated literals.

### Examples

```ruby
# bad

"result is #{10}"
```
```ruby
# good

"result is 10"
```

## Lint/Loop

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.9 | -

This cop checks for uses of *begin...end while/until something*.

### Examples

```ruby
# bad

# using while
begin
  do_something
end while some_condition
```
```ruby
# bad

# using until
begin
  do_something
end until some_condition
```
```ruby
# good

# while replacement
loop do
  do_something
  break unless some_condition
end
```
```ruby
# good

# until replacement
loop do
  do_something
  break if some_condition
end
```

### References

* [https://rubystyle.guide#loop-with-break](https://rubystyle.guide#loop-with-break)

## Lint/MissingCopEnableDirective

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.52 | -

This cop checks that there is an `# rubocop:enable ...` statement
after a `# rubocop:disable ...` statement. This will prevent leaving
cop disables on wide ranges of code, that latter contributors to
a file wouldn't be aware of.

### Examples

```ruby
# Lint/MissingCopEnableDirective:
#   MaximumRangeSize: .inf

# good
# rubocop:disable Layout/SpaceAroundOperators
x= 0
# rubocop:enable Layout/SpaceAroundOperators
# y = 1
# EOF

# bad
# rubocop:disable Layout/SpaceAroundOperators
x= 0
# EOF
```
```ruby
# Lint/MissingCopEnableDirective:
#   MaximumRangeSize: 2

# good
# rubocop:disable Layout/SpaceAroundOperators
x= 0
# With the previous, there are 2 lines on which cop is disabled.
# rubocop:enable Layout/SpaceAroundOperators

# bad
# rubocop:disable Layout/SpaceAroundOperators
x= 0
x += 1
# Including this, that's 3 lines on which the cop is disabled.
# rubocop:enable Layout/SpaceAroundOperators
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
MaximumRangeSize | `Infinity` | Float

## Lint/MixedRegexpCaptureTypes

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Pending | Yes | No | 0.85 | -

Do not mix named captures and numbered captures in a Regexp literal
because numbered capture is ignored if they're mixed.
Replace numbered captures with non-capturing groupings or
named captures.

  # bad
  /(?<foo>FOO)(BAR)/

  # good
  /(?<foo>FOO)(?<bar>BAR)/

  # good
  /(?<foo>FOO)(?:BAR)/

  # good
  /(FOO)(BAR)/

## Lint/MultipleComparison

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.47 | 0.77

In math and Python, we can use `x < y < z` style comparison to compare
multiple value. However, we can't use the comparison in Ruby. However,
the comparison is not syntax error. This cop checks the bad usage of
comparison operators.

### Examples

```ruby
# bad

x < y < z
10 <= x <= 20
```
```ruby
# good

x < y && y < z
10 <= x && x <= 20
```

## Lint/NestedMethodDefinition

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.32 | -

This cop checks for nested method definitions.

### Examples

```ruby
# bad

# `bar` definition actually produces methods in the same scope
# as the outer `foo` method. Furthermore, the `bar` method
# will be redefined every time `foo` is invoked.
def foo
  def bar
  end
end
```
```ruby
# good

def foo
  bar = -> { puts 'hello' }
  bar.call
end
```
```ruby
# good

def foo
  self.class.class_eval do
    def bar
    end
  end
end

def foo
  self.class.module_exec do
    def bar
    end
  end
end
```
```ruby
# good

def foo
  class << self
    def bar
    end
  end
end
```

### References

* [https://rubystyle.guide#no-nested-methods](https://rubystyle.guide#no-nested-methods)

## Lint/NestedPercentLiteral

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.52 | -

This cop checks for nested percent literals.

### Examples

```ruby
# bad

# The percent literal for nested_attributes is parsed as four tokens,
# yielding the array [:name, :content, :"%i[incorrectly", :"nested]"].
attributes = {
  valid_attributes: %i[name content],
  nested_attributes: %i[name content %i[incorrectly nested]]
}
```

## Lint/NextWithoutAccumulator

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.36 | -

Don't omit the accumulator when calling `next` in a `reduce` block.

### Examples

```ruby
# bad

result = (1..4).reduce(0) do |acc, i|
  next if i.odd?
  acc + i
end
```
```ruby
# good

result = (1..4).reduce(0) do |acc, i|
  next acc if i.odd?
  acc + i
end
```

## Lint/NonDeterministicRequireOrder

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | No | Yes (Unsafe) | 0.78 | -

`Dir[...]` and `Dir.glob(...)` do not make any guarantees about
the order in which files are returned. The final order is
determined by the operating system and file system.
This means that using them in cases where the order matters,
such as requiring files, can lead to intermittent failures
that are hard to debug. To ensure this doesn't happen,
always sort the list.

### Examples

```ruby
# bad
Dir["./lib/**/*.rb"].each do |file|
  require file
end

# good
Dir["./lib/**/*.rb"].sort.each do |file|
  require file
end
```
```ruby
# bad
Dir.glob(Rails.root.join(__dir__, 'test', '*.rb')) do |file|
  require file
end

# good
Dir.glob(Rails.root.join(__dir__, 'test', '*.rb')).sort.each do |file|
  require file
end
```

## Lint/NonLocalExitFromIterator

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.30 | -

This cop checks for non-local exits from iterators without a return
value. It registers an offense under these conditions:

 - No value is returned,
 - the block is preceded by a method chain,
 - the block has arguments,
 - the method which receives the block is not `define_method`
   or `define_singleton_method`,
 - the return is not contained in an inner scope, e.g. a lambda or a
   method definition.

### Examples

```ruby
class ItemApi
  rescue_from ValidationError do |e| # non-iteration block with arg
    return { message: 'validation error' } unless e.errors # allowed
    error_array = e.errors.map do |error| # block with method chain
      return if error.suppress? # warned
      return "#{error.param}: invalid" unless error.message # allowed
      "#{error.param}: #{error.message}"
    end
    { message: 'validation error', errors: error_array }
  end

  def update_items
    transaction do # block without arguments
      return unless update_necessary? # allowed
      find_each do |item| # block without method chain
        return if item.stock == 0 # false-negative...
        item.update!(foobar: true)
      end
    end
  end
end
```

## Lint/NumberConversion

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Disabled | Yes | Yes (Unsafe) | 0.53 | 0.70

This cop warns the usage of unsafe number conversions. Unsafe
number conversion can cause unexpected error if auto type conversion
fails. Cop prefer parsing with number class instead.

### Examples

```ruby
# bad

'10'.to_i
'10.2'.to_f
'10'.to_c

# good

Integer('10', 10)
Float('10.2')
Complex('10')
```

## Lint/OrderedMagicComments

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.53 | -

Checks the proper ordering of magic comments and whether
a magic comment is not placed before a shebang.

### Examples

```ruby
# bad

# frozen_string_literal: true
# encoding: ascii
p [''.frozen?, ''.encoding] #=> [true, #<Encoding:UTF-8>]

# good

# encoding: ascii
# frozen_string_literal: true
p [''.frozen?, ''.encoding] #=> [true, #<Encoding:US-ASCII>]

# good

#!/usr/bin/env ruby
# encoding: ascii
# frozen_string_literal: true
p [''.frozen?, ''.encoding] #=> [true, #<Encoding:US-ASCII>]
```

## Lint/ParenthesesAsGroupedExpression

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.12 | 0.83

Checks for space between the name of a called method and a left
parenthesis.

### Examples

```ruby
# bad
do_something (foo)

# good
do_something(foo)
do_something (2 + 3) * 4
do_something (foo * bar).baz
```

### References

* [https://rubystyle.guide#parens-no-spaces](https://rubystyle.guide#parens-no-spaces)

## Lint/PercentStringArray

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | No | Yes (Unsafe) | 0.41 | -

This cop checks for quotes and commas in %w, e.g. `%w('foo', "bar")`

It is more likely that the additional characters are unintended (for
example, mistranslating an array of literals to percent string notation)
rather than meant to be part of the resulting strings.

### Examples

```ruby
# bad

%w('foo', "bar")
```
```ruby
# good

%w(foo bar)
```

## Lint/PercentSymbolArray

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.41 | -

This cop checks for colons and commas in %i, e.g. `%i(:foo, :bar)`

It is more likely that the additional characters are unintended (for
example, mistranslating an array of literals to percent string notation)
rather than meant to be part of the resulting symbols.

### Examples

```ruby
# bad

%i(:foo, :bar)
```
```ruby
# good

%i(foo bar)
```

## Lint/RaiseException

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Pending | Yes | No | 0.81 | -

This cop checks for `raise` or `fail` statements which are
raising `Exception` class.

You can specify a module name that will be an implicit namespace
using `AllowedImplicitNamespaces` option. The cop cause a false positive
for namespaced `Exception` when a namespace is omitted. This option can
prevent the false positive by specifying a namespace to be omitted for
`Exception`. Alternatively, make `Exception` a fully qualified class
name with an explicit namespace.

### Examples

```ruby
# bad
raise Exception, 'Error message here'

# good
raise StandardError, 'Error message here'
```
#### AllowedImplicitNamespaces: ['Gem']

```ruby
# good
module Gem
  def self.foo
    raise Exception # This exception means `Gem::Exception`.
  end
end
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
AllowedImplicitNamespaces | `Gem` | Array

### References

* [https://rubystyle.guide#raise-exception](https://rubystyle.guide#raise-exception)

## Lint/RandOne

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.36 | -

This cop checks for `rand(1)` calls.
Such calls always return `0`.

### Examples

```ruby
# bad

rand 1
Kernel.rand(-1)
rand 1.0
rand(-1.0)
```
```ruby
# good

0 # just use 0 instead
```

## Lint/RedundantCopDisableDirective

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.76 | -

This cop detects instances of rubocop:disable comments that can be
removed without causing any offenses to be reported. It's implemented
as a cop in that it inherits from the Cop base class and calls
add_offense. The unusual part of its implementation is that it doesn't
have any on_* methods or an investigate method. This means that it
doesn't take part in the investigation phase when the other cops do
their work. Instead, it waits until it's called in a later stage of the
execution. The reason it can't be implemented as a normal cop is that
it depends on the results of all other cops to do its work.

### Examples

```ruby
# bad
# rubocop:disable Layout/LineLength
x += 1
# rubocop:enable Layout/LineLength

# good
x += 1
```

## Lint/RedundantCopEnableDirective

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.76 | -

This cop detects instances of rubocop:enable comments that can be
removed.

When comment enables all cops at once `rubocop:enable all`
that cop checks whether any cop was actually enabled.

### Examples

```ruby
# bad
foo = 1
# rubocop:enable Layout/LineLength

# good
foo = 1
```
```ruby
# bad
# rubocop:disable Style/StringLiterals
foo = "1"
# rubocop:enable Style/StringLiterals
baz
# rubocop:enable all

# good
# rubocop:disable Style/StringLiterals
foo = "1"
# rubocop:enable all
baz
```

## Lint/RedundantRequireStatement

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.76 | -

Checks for unnecessary `require` statement.

The following features are unnecessary `require` statement because
they are already loaded.

ruby -ve 'p $LOADED_FEATURES.reject { |feature| %r|/| =~ feature }'
ruby 2.2.8p477 (2017-09-14 revision 59906) [x86_64-darwin13]
["enumerator.so", "rational.so", "complex.so", "thread.rb"]

This cop targets Ruby 2.2 or higher containing these 4 features.

### Examples

```ruby
# bad
require 'unloaded_feature'
require 'thread'

# good
require 'unloaded_feature'
```

## Lint/RedundantSplatExpansion

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.76 | -

This cop checks for unneeded usages of splat expansion

### Examples

```ruby
# bad

a = *[1, 2, 3]
a = *'a'
a = *1

begin
  foo
rescue *[StandardError, ApplicationError]
  bar
end

case foo
when *[1, 2, 3]
  bar
else
  baz
end
```
```ruby
# good

c = [1, 2, 3]
a = *c
a, b = *c
a, *b = *c
a = *1..10
a = ['a']

begin
  foo
rescue StandardError, ApplicationError
  bar
end

case foo
when 1, 2, 3
  bar
else
  baz
end
```

## Lint/RedundantStringCoercion

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.19 | 0.77

This cop checks for string conversion in string interpolation,
which is redundant.

### Examples

```ruby
# bad

"result is #{something.to_s}"
```
```ruby
# good

"result is #{something}"
```

### References

* [https://rubystyle.guide#no-to-s](https://rubystyle.guide#no-to-s)

## Lint/RedundantWithIndex

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.50 | -

This cop checks for redundant `with_index`.

### Examples

```ruby
# bad
ary.each_with_index do |v|
  v
end

# good
ary.each do |v|
  v
end

# bad
ary.each.with_index do |v|
  v
end

# good
ary.each do |v|
  v
end
```

## Lint/RedundantWithObject

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.51 | -

This cop checks for redundant `with_object`.

### Examples

```ruby
# bad
ary.each_with_object([]) do |v|
  v
end

# good
ary.each do |v|
  v
end

# bad
ary.each.with_object([]) do |v|
  v
end

# good
ary.each do |v|
  v
end
```

## Lint/RegexpAsCondition

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.51 | -

This cop checks for regexp literals used as `match-current-line`.
If a regexp literal is in condition, the regexp matches `$_` implicitly.

### Examples

```ruby
# bad
if /foo/
  do_something
end

# good
if /foo/ =~ $_
  do_something
end
```

## Lint/RequireParentheses

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.18 | -

This cop checks for expressions where there is a call to a predicate
method with at least one argument, where no parentheses are used around
the parameter list, and a boolean operator, && or ||, is used in the
last argument.

The idea behind warning for these constructs is that the user might
be under the impression that the return value from the method call is
an operand of &&/||.

### Examples

```ruby
# bad

if day.is? :tuesday && month == :jan
  # ...
end
```
```ruby
# good

if day.is?(:tuesday) && month == :jan
  # ...
end
```

## Lint/RescueException

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.9 | 0.27.1

This cop checks for *rescue* blocks targeting the Exception class.

### Examples

```ruby
# bad

begin
  do_something
rescue Exception
  handle_exception
end
```
```ruby
# good

begin
  do_something
rescue ArgumentError
  handle_exception
end
```

### References

* [https://rubystyle.guide#no-blind-rescues](https://rubystyle.guide#no-blind-rescues)

## Lint/RescueType

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.49 | -

Check for arguments to `rescue` that will result in a `TypeError`
if an exception is raised.

### Examples

```ruby
# bad
begin
  bar
rescue nil
  baz
end

# bad
def foo
  bar
rescue 1, 'a', "#{b}", 0.0, [], {}
  baz
end

# good
begin
  bar
rescue
  baz
end

# good
def foo
  bar
rescue NameError
  baz
end
```

## Lint/ReturnInVoidContext

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.50 | -

This cop checks for the use of a return with a value in a context
where the value will be ignored. (initialize and setter methods)

### Examples

```ruby
# bad
def initialize
  foo
  return :qux if bar?
  baz
end

def foo=(bar)
  return 42
end
```
```ruby
# good
def initialize
  foo
  return if bar?
  baz
end

def foo=(bar)
  return
end
```

## Lint/SafeNavigationChain

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.47 | 0.77

The safe navigation operator returns nil if the receiver is
nil. If you chain an ordinary method call after a safe
navigation operator, it raises NoMethodError. We should use a
safe navigation operator after a safe navigation operator.
This cop checks for the problem outlined above.

### Examples

```ruby
# bad

x&.foo.bar
x&.foo + bar
x&.foo[bar]
```
```ruby
# good

x&.foo&.bar
x&.foo || bar
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
AllowedMethods | `present?`, `blank?`, `presence`, `try`, `try!` | Array

## Lint/SafeNavigationConsistency

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.55 | 0.77

This cop check to make sure that if safe navigation is used for a method
call in an `&&` or `||` condition that safe navigation is used for all
method calls on that same object.

### Examples

```ruby
# bad
foo&.bar && foo.baz

# bad
foo.bar || foo&.baz

# bad
foo&.bar && (foobar.baz || foo.baz)

# good
foo.bar && foo.baz

# good
foo&.bar || foo&.baz

# good
foo&.bar && (foobar.baz || foo&.baz)
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
AllowedMethods | `present?`, `blank?`, `presence`, `try`, `try!` | Array

## Lint/SafeNavigationWithEmpty

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.62 | -

This cop checks to make sure safe navigation isn't used with `empty?` in
a conditional.

While the safe navigation operator is generally a good idea, when
checking `foo&.empty?` in a conditional, `foo` being `nil` will actually
do the opposite of what the author intends.

### Examples

```ruby
# bad
return if foo&.empty?
return unless foo&.empty?

# good
return if foo && foo.empty?
return unless foo && foo.empty?
```

## Lint/ScriptPermission

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.49 | 0.50

This cop checks if a file which has a shebang line as
its first line is granted execute permission.

### Examples

```ruby
# bad

# A file which has a shebang line as its first line is not
# granted execute permission.

#!/usr/bin/env ruby
puts 'hello, world'

# good

# A file which has a shebang line as its first line is
# granted execute permission.

#!/usr/bin/env ruby
puts 'hello, world'

# good

# A file which has not a shebang line as its first line is not
# granted execute permission.

puts 'hello, world'
```

## Lint/SendWithMixinArgument

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.75 | -

This cop checks for `send`, `public_send`, and `__send__` methods
when using mix-in.

`include` and `prepend` methods were private methods until Ruby 2.0,
they were mixed-in via `send` method. This cop uses Ruby 2.1 or
higher style that can be called by public methods.
And `extend` method that was originally a public method is also targeted
for style unification.

### Examples

```ruby
# bad
Foo.send(:include, Bar)
Foo.send(:prepend, Bar)
Foo.send(:extend, Bar)

# bad
Foo.public_send(:include, Bar)
Foo.public_send(:prepend, Bar)
Foo.public_send(:extend, Bar)

# bad
Foo.__send__(:include, Bar)
Foo.__send__(:prepend, Bar)
Foo.__send__(:extend, Bar)

# good
Foo.include Bar
Foo.prepend Bar
Foo.extend Bar
```

## Lint/ShadowedArgument

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.52 | -

This cop checks for shadowed arguments.

This cop has `IgnoreImplicitReferences` configuration option.
It means argument shadowing is used in order to pass parameters
to zero arity `super` when `IgnoreImplicitReferences` is `true`.

### Examples

```ruby
# bad
do_something do |foo|
  foo = 42
  puts foo
end

def do_something(foo)
  foo = 42
  puts foo
end

# good
do_something do |foo|
  foo = foo + 42
  puts foo
end

def do_something(foo)
  foo = foo + 42
  puts foo
end

def do_something(foo)
  puts foo
end
```
#### IgnoreImplicitReferences: false (default)

```ruby
# bad
def do_something(foo)
  foo = 42
  super
end

def do_something(foo)
  foo = super
  bar
end
```
#### IgnoreImplicitReferences: true

```ruby
# good
def do_something(foo)
  foo = 42
  super
end

def do_something(foo)
  foo = super
  bar
end
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
IgnoreImplicitReferences | `false` | Boolean

## Lint/ShadowedException

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.41 | -

This cop checks for a rescued exception that get shadowed by a
less specific exception being rescued before a more specific
exception is rescued.

### Examples

```ruby
# bad

begin
  something
rescue Exception
  handle_exception
rescue StandardError
  handle_standard_error
end

# good

begin
  something
rescue StandardError
  handle_standard_error
rescue Exception
  handle_exception
end

# good, however depending on runtime environment.
#
# This is a special case for system call errors.
# System dependent error code depends on runtime environment.
# For example, whether `Errno::EAGAIN` and `Errno::EWOULDBLOCK` are
# the same error code or different error code depends on environment.
# This good case is for `Errno::EAGAIN` and `Errno::EWOULDBLOCK` with
# the same error code.
begin
  something
rescue Errno::EAGAIN, Errno::EWOULDBLOCK
  handle_standard_error
end
```

## Lint/ShadowingOuterLocalVariable

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.9 | -

This cop looks for use of the same name as outer local variables
for block arguments or block local variables.
This is a mimic of the warning
"shadowing outer local variable - foo" from `ruby -cw`.

### Examples

```ruby
# bad

def some_method
  foo = 1

  2.times do |foo| # shadowing outer `foo`
    do_something(foo)
  end
end
```
```ruby
# good

def some_method
  foo = 1

  2.times do |bar|
    do_something(bar)
  end
end
```

## Lint/StructNewOverride

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Pending | Yes | No | 0.81 | -

This cop checks unexpected overrides of the `Struct` built-in methods
via `Struct.new`.

### Examples

```ruby
# bad
Bad = Struct.new(:members, :clone, :count)
b = Bad.new([], true, 1)
b.members #=> [] (overriding `Struct#members`)
b.clone #=> true (overriding `Object#clone`)
b.count #=> 1 (overriding `Enumerable#count`)

# good
Good = Struct.new(:id, :name)
g = Good.new(1, "foo")
g.members #=> [:id, :name]
g.clone #=> #<struct Good id=1, name="foo">
g.count #=> 2
```

## Lint/SuppressedException

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.9 | 0.81

This cop checks for *rescue* blocks with no body.

### Examples

```ruby
# bad
def some_method
  do_something
rescue
end

# bad
begin
  do_something
rescue
end

# good
def some_method
  do_something
rescue
  handle_exception
end

# good
begin
  do_something
rescue
  handle_exception
end
```
#### AllowComments: true (default)

```ruby
# good
def some_method
  do_something
rescue
  # do nothing
end

# good
begin
  do_something
rescue
  # do nothing
end
```
#### AllowComments: false

```ruby
# bad
def some_method
  do_something
rescue
  # do nothing
end

# bad
begin
  do_something
rescue
  # do nothing
end
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
AllowComments | `true` | Boolean

### References

* [https://rubystyle.guide#dont-hide-exceptions](https://rubystyle.guide#dont-hide-exceptions)

## Lint/Syntax

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.9 | -

This is not actually a cop. It does not inspect anything. It just
provides methods to repack Parser's diagnostics/errors
into RuboCop's offenses.

## Lint/ToJSON

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.66 | -

This cop checks to make sure `#to_json` includes an optional argument.
When overriding `#to_json`, callers may invoke JSON
generation via `JSON.generate(your_obj)`.  Since `JSON#generate` allows
for an optional argument, your method should too.

### Examples

```ruby
# bad
def to_json
end

# good
def to_json(*_args)
end
```

## Lint/UnderscorePrefixedVariableName

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.21 | -

This cop checks for underscore-prefixed variables that are actually
used.

Since block keyword arguments cannot be arbitrarily named at call
sites, the `AllowKeywordBlockArguments` will allow use of underscore-
prefixed block keyword arguments.

### Examples

#### AllowKeywordBlockArguments: false (default)

```ruby
# bad

[1, 2, 3].each do |_num|
  do_something(_num)
end

query(:sales) do |_id:, revenue:, cost:|
  {_id: _id, profit: revenue - cost}
end

# good

[1, 2, 3].each do |num|
  do_something(num)
end

[1, 2, 3].each do |_num|
  do_something # not using `_num`
end
```
#### AllowKeywordBlockArguments: true

```ruby
# good

query(:sales) do |_id:, revenue:, cost:|
  {_id: _id, profit: revenue - cost}
end
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
AllowKeywordBlockArguments | `false` | Boolean

## Lint/UnifiedInteger

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.43 | -

This cop checks for using Fixnum or Bignum constant.

### Examples

```ruby
# bad

1.is_a?(Fixnum)
1.is_a?(Bignum)
```
```ruby
# good

1.is_a?(Integer)
```

## Lint/UnreachableCode

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.9 | -

This cop checks for unreachable code.
The check are based on the presence of flow of control
statement in non-final position in *begin*(implicit) blocks.

### Examples

```ruby
# bad

def some_method
  return
  do_something
end

# bad

def some_method
  if cond
    return
  else
    return
  end
  do_something
end
```
```ruby
# good

def some_method
  do_something
end
```

## Lint/UnusedBlockArgument

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.21 | 0.22

This cop checks for unused block arguments.

### Examples

```ruby
# bad
do_something do |used, unused|
  puts used
end

do_something do |bar|
  puts :foo
end

define_method(:foo) do |bar|
  puts :baz
end

# good
do_something do |used, _unused|
  puts used
end

do_something do
  puts :foo
end

define_method(:foo) do |_bar|
  puts :baz
end
```
#### IgnoreEmptyBlocks: true (default)

```ruby
# good
do_something { |unused| }
```
#### IgnoreEmptyBlocks: false

```ruby
# bad
do_something { |unused| }
```
#### AllowUnusedKeywordArguments: false (default)

```ruby
# bad
do_something do |unused: 42|
  foo
end
```
#### AllowUnusedKeywordArguments: true

```ruby
# good
do_something do |unused: 42|
  foo
end
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
IgnoreEmptyBlocks | `true` | Boolean
AllowUnusedKeywordArguments | `false` | Boolean

### References

* [https://rubystyle.guide#underscore-unused-vars](https://rubystyle.guide#underscore-unused-vars)

## Lint/UnusedMethodArgument

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.21 | 0.81

This cop checks for unused method arguments.

### Examples

```ruby
# bad
def some_method(used, unused, _unused_but_allowed)
  puts used
end

# good
def some_method(used, _unused, _unused_but_allowed)
  puts used
end
```
#### AllowUnusedKeywordArguments: false (default)

```ruby
# bad
def do_something(used, unused: 42)
  used
end
```
#### AllowUnusedKeywordArguments: true

```ruby
# good
def do_something(used, unused: 42)
  used
end
```
#### IgnoreEmptyMethods: true (default)

```ruby
# good
def do_something(unused)
end
```
#### IgnoreEmptyMethods: false

```ruby
# bad
def do_something(unused)
end
```
#### IgnoreNotImplementedMethods: true (default)

```ruby
# good
def do_something(unused)
  raise NotImplementedError
end

def do_something_else(unused)
  fail "TODO"
end
```
#### IgnoreNotImplementedMethods: false

```ruby
# bad
def do_something(unused)
  raise NotImplementedError
end

def do_something_else(unused)
  fail "TODO"
end
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
AllowUnusedKeywordArguments | `false` | Boolean
IgnoreEmptyMethods | `true` | Boolean
IgnoreNotImplementedMethods | `true` | Boolean

### References

* [https://rubystyle.guide#underscore-unused-vars](https://rubystyle.guide#underscore-unused-vars)

## Lint/UriEscapeUnescape

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.50 | -

This cop identifies places where `URI.escape` can be replaced by
`CGI.escape`, `URI.encode_www_form`, or `URI.encode_www_form_component`
depending on your specific use case.
Also this cop identifies places where `URI.unescape` can be replaced by
`CGI.unescape`, `URI.decode_www_form`,
or `URI.decode_www_form_component` depending on your specific use case.

### Examples

```ruby
# bad
URI.escape('http://example.com')
URI.encode('http://example.com')

# good
CGI.escape('http://example.com')
URI.encode_www_form([['example', 'param'], ['lang', 'en']])
URI.encode_www_form(page: 10, locale: 'en')
URI.encode_www_form_component('http://example.com')

# bad
URI.unescape(enc_uri)
URI.decode(enc_uri)

# good
CGI.unescape(enc_uri)
URI.decode_www_form(enc_uri)
URI.decode_www_form_component(enc_uri)
```

## Lint/UriRegexp

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.50 | -

This cop identifies places where `URI.regexp` is obsolete and should
not be used. Instead, use `URI::DEFAULT_PARSER.make_regexp`.

### Examples

```ruby
# bad
URI.regexp('http://example.com')

# good
URI::DEFAULT_PARSER.make_regexp('http://example.com')
```

## Lint/UselessAccessModifier

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | Yes  | 0.20 | 0.83

This cop checks for redundant access modifiers, including those with no
code, those which are repeated, and leading `public` modifiers in a
class or module body. Conditionally-defined methods are considered as
always being defined, and thus access modifiers guarding such methods
are not redundant.

This cop has `ContextCreatingMethods` option. The default setting value
is an empty array that means no method is specified.
This setting is an array of methods which, when called, are known to
create its own context in the module's current access context.

It also has `MethodCreatingMethods` option. The default setting value
is an empty array that means no method is specified.
This setting is an array of methods which, when called, are known to
create other methods in the module's current access context.

### Examples

```ruby
# bad
class Foo
  public # this is redundant (default access is public)

  def method
  end
end

# bad
class Foo
  # The following is redundant (methods defined on the class'
  # singleton class are not affected by the public modifier)
  public

  def self.method3
  end
end

# bad
class Foo
  protected

  define_method(:method2) do
  end

  protected # this is redundant (repeated from previous modifier)

  [1,2,3].each do |i|
    define_method("foo#{i}") do
    end
  end
end

# bad
class Foo
  private # this is redundant (no following methods are defined)
end

# good
class Foo
  private # this is not redundant (a method is defined)

  def method2
  end
end

# good
class Foo
  # The following is not redundant (conditionally defined methods are
  # considered as always defining a method)
  private

  if condition?
    def method
    end
  end
end

# good
class Foo
  protected # this is not redundant (a method is defined)

  define_method(:method2) do
  end
end
```
#### ContextCreatingMethods: concerning

```ruby
# Lint/UselessAccessModifier:
#   ContextCreatingMethods:
#     - concerning

# good
require 'active_support/concern'
class Foo
  concerning :Bar do
    def some_public_method
    end

    private

    def some_private_method
    end
  end

  # this is not redundant because `concerning` created its own context
  private

  def some_other_private_method
  end
end
```
#### MethodCreatingMethods: delegate

```ruby
# Lint/UselessAccessModifier:
#   MethodCreatingMethods:
#     - delegate

# good
require 'active_support/core_ext/module/delegation'
class Foo
  # this is not redundant because `delegate` creates methods
  private

  delegate :method_a, to: :method_b
end
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
ContextCreatingMethods | `[]` | Array
MethodCreatingMethods | `[]` | Array

## Lint/UselessAssignment

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.11 | -

This cop checks for every useless assignment to local variable in every
scope.
The basic idea for this cop was from the warning of `ruby -cw`:

  assigned but unused variable - foo

Currently this cop has advanced logic that detects unreferenced
reassignments and properly handles varied cases such as branch, loop,
rescue, ensure, etc.

### Examples

```ruby
# bad

def some_method
  some_var = 1
  do_something
end
```
```ruby
# good

def some_method
  some_var = 1
  do_something(some_var)
end
```

### References

* [https://rubystyle.guide#underscore-unused-vars](https://rubystyle.guide#underscore-unused-vars)

## Lint/UselessComparison

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.11 | -

This cop checks for comparison of something with itself.

### Examples

```ruby
# bad

x.top >= x.top
```

## Lint/UselessElseWithoutRescue

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.17 | -

This cop checks for useless `else` in `begin..end` without `rescue`.

Note: This syntax is no longer valid on Ruby 2.6 or higher and
this cop is going to be removed at some point the future.

### Examples

```ruby
# bad

begin
  do_something
else
  do_something_else # This will never be run.
end
```
```ruby
# good

begin
  do_something
rescue
  handle_errors
else
  do_something_else
end
```

## Lint/UselessSetterCall

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | No | No | 0.13 | 0.80

This cop checks for setter call to local variable as the final
expression of a function definition.

Note: There are edge cases in which the local variable references a
value that is also accessible outside the local scope. This is not
detected by the cop, and it can yield a false positive.

### Examples

```ruby
# bad

def something
  x = Something.new
  x.attr = 5
end
```
```ruby
# good

def something
  x = Something.new
  x.attr = 5
  x
end
```

## Lint/Void

Enabled by default | Safe | Supports autocorrection | VersionAdded | VersionChanged
--- | --- | --- | --- | ---
Enabled | Yes | No | 0.9 | -

This cop checks for operators, variables, literals, and nonmutating
methods used in void context.

### Examples

#### CheckForMethodsWithNoSideEffects: false (default)

```ruby
# bad
def some_method
  some_num * 10
  do_something
end

def some_method(some_var)
  some_var
  do_something
end
```
#### CheckForMethodsWithNoSideEffects: true

```ruby
# bad
def some_method(some_array)
  some_array.sort
  do_something(some_array)
end

# good
def some_method
  do_something
  some_num * 10
end

def some_method(some_var)
  do_something
  some_var
end

def some_method(some_array)
  some_array.sort!
  do_something(some_array)
end
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
CheckForMethodsWithNoSideEffects | `false` | Boolean

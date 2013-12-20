# Ruby Style Guide

- Style
- Example
- Testing
- Version Switching

## Style

- Use `snake_case` for methods and variables.
- Use `CamelCase` for classes and modules (Keep acronyms like HTTP, RFC, XML uppercase).
- Use `SCREAMING_SNAKE_CASE` for other constants.
- Use four spaces for indenting your code (do not use tabs).
- Restrict code to 80 characters per line.
- Remove trailing whitespace.
- Use spaces around operators, after commas, colons and semicolons, around `{` and before `}`.
- No spaces after `(`, `[` or before `]`, `)`.
- No spaces after `!`.
- Use appropriate spacing between lines of code, group lines of code together if they are related and are short enough to fit onto one line each. Just don't go crazy with too little or too much space.
- Use `def` with parentheses when there are arguments. Omit the parentheses when the method doesn't accept any arguments.
- Never use `then` for multi-line `if/unless`.
- Avoid `for/while` loops unless you know what you're doing. Enumerables/Iterators such as `each`, `map`, `reduce` etc should be what you use instead.
- Don't use the `and` or `or` keywords. Always use `&&` and `||` instead.
- With a multi-line if statement, if the body is one line then just use a single line if (see example below).
- Try to make sure your opening if condition is a positive and not a negative (makes it easier to read the flow).
- Don't use parentheses around the condition of an `if/unless/while`.
- Prefer `{...}` over `do...end` for single-line blocks.
- Avoid using `{...}` for multi-line blocks.
- Always use `do...end` for "control flow" and "method definitions" (e.g. in Rakefiles and certain DSLs).
- Don't use a `return` command unless you need to (in Ruby the last expression is what gets returned).
- Use spaces around the `=` operator when assigning default values to method parameters.
- Use `||=` freely to initialize variables.
- Don't use `||=` to initialize boolean variables (see example below)
- Avoid using Perl-style special variables (like `$0-9`, `$:` etc). They are quite cryptic and their use in anything but one-liner scripts is discouraged. Prefer long form versions such as `$PROGRAM_NAME`.
- Never put a space between a method name and the opening parenthesis.
- Use _ for unused block parameters (see example below).
- Don't use the `===` operator to check types (unlike in JS where it's a good thing). **NOTE** use duck-typing instead of explicitly checking the type of an object (which is a code smell). `===` is mostly an implementation detail to support Ruby features like `case`. For example, `String === "hi"` is `true` and `"hi" === String` is `false`. Instead, use `is_a?` or `kind_of?` *if you must*.
- The names of predicate methods (methods that return a boolean value) should end in a question mark. (i.e. `Array#empty?`).
- The names of potentially "dangerous" methods (i.e. methods that modify `self` or the arguments, `exit!`, etc.) should end with an exclamation mark.
- Prefer the use of Class 'instance' variables `@variable` over Class variables `@@variable`.
- Use Symbols `:name` over Strings `'name'` wherever possible.
- Use `Set` instead of `Array` when dealing with unique elements. `Set` implements a collection of unordered values with no duplicates. This is a hybrid of `Array`'s intuitive inter-operation facilities and `Hash`'s fast lookup.
- Prefer String interpolation over concatenation (see example below).
- Prefer double quoted Strings `"text"` to single quotes `'text'` as interpolation will always work without needing to change quotation style.
- Use literals (such as `%w(a b c)` === `['a', 'b', 'c']`) where appropriate.
- Use hashrocket syntax for Hash literals instead of the JSON style (see example below).

## Example

```ruby
sum = 1 + 2
a, b = 1, 2
1 > 2 ? true : false; puts "Hi"
[1, 2, 3].each { |e| puts e }

some(arg).other
[1, 2, 3].length

!array.include?(element)

def some_method
  # code
end

def some_method_with_arguments(arg1, arg2)
  # code
end

# good (doesn't use `then`)
if some_condition
  # code
end

# good (the body is one line and so it doesn't need to be a multi-line `if` statement)
do_something if some_condition

# good (positive condition comes first)
if success?
  puts "success"
else
  puts "failure"
end

# good (no parenthesis around condition)
if x > 10
  # code
end

# good (uses braces for single line blocks)
names.each { |name| puts name }

# good (uses `do...end` for multi-line blocks)
names.each do |name|
  # multiple
  # lines of
  # code
end

# good (spaces around the `=` for default values)
def some_method(arg1 = :default, arg2 = nil, arg3 = [])
  # code
end

# set name to foo, only if it's `nil` or `false`
name ||= "foo"

# good (checks for `nil` rather than using coercion)
# if we really wanted the value to be `false` then coercion would cause the value always to be set to `true`
enabled = true if enabled.nil?

# good (used _ for the unused block parameter)
result = hash.map { |_, value| value + 1 }

# good (uses interpolation and not concatenation)
email_with_name = "#{user.name} <#{user.email}>"

# good (uses hashrocket style)
user = {
  :login => "defunkt",
  :name => "Chris Wanstrath",
  "followers-count" => 52390235
}
```

## Testing

For unit testing your Ruby code we recommend either [RSpec](http://rspec.info/) or [Cucumber](http://cukes.info/).

## Version Switching

There are two popular Ruby version managers...

- [chruby](https://github.com/postmodern/chruby#readme)
- [rvm](https://rvm.io/)

...and two popular Ruby installers...

- [ruby-install](https://github.com/postmodern/ruby-install#readme)
- [ruby-build](https://github.com/sstephenson/ruby-build#readme)

We recommend using [chruby](https://github.com/postmodern/chruby#readme) and [ruby-install](https://github.com/postmodern/ruby-install#readme).

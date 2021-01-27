# Exceptions And Errors
- `Exception` is the base class.
- All Error types, including `StandardError`, inherit from `Exception`.
- You shouldn't simply rescue `Exception`, because:
  - It will swallow *every* type of error, and some of them are dangerous.
  - eg: `SignalException::Interrupt` allows you to close an app by hitting ctrl-c. If you rescue this error it won't close!
  - eg: `ScriptError::SyntaxError` 

2 global variables for accessing unhandled exceptions:
- `$!`: the current unhandled exception (if it exists)
- `$@`: the backtrace for the current unhandled exception (if it exists)

## Rescue block
Basic syntax:
```
begin
  do_something()
rescue => e
  puts e # e is an exception object containing info about the error. 
end

```
- The default argument is `StandardError` (when no Error is specified by passing an argument to `rescue`).
- `rescue` will rescue the passed `Error`, AND any of its subclasses.
- the `=> e` line takes the rescued error, and saves it to the variable `e`.
  - this gives you access to the error in the `rescue` block (eg for printing it)
  
## Raise
- Raises an `Exception`
- By default it raises a `RuntimeError` instance
- You can specify a different Error, or a message:
```ruby
raise # Raises a RuntimeError with no message
raise "Failed to create socket" # Raises a RuntimeError with the string as a message
raise ArgumentError, "No parameters" # Raises an ArgumentError with the string as a message
raise MyCustomError # This only works if you've already defined MyCustomError 

```
- You can't raise a custom Error until you've defined the Error class though
- is defined on `Kernel`
  
## Sources
- [Exception in the Ruby docs](https://ruby-doc.org/core-2.5.1/Exception.html)


## Exception Hierachy
```
Exception
 NoMemoryError
 ScriptError
   LoadError
   NotImplementedError
   SyntaxError
 SignalException
   Interrupt
 StandardError
   ArgumentError
   IOError
     EOFError
   IndexError
   LocalJumpError
   NameError
     NoMethodError
   RangeError
     FloatDomainError
   RegexpError
   RuntimeError
   SecurityError
   SystemCallError
   SystemStackError
   ThreadError
   TypeError
   ZeroDivisionError
 SystemExit
 fatal
```

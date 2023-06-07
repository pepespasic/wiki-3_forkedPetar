# The Elixir Programming Language

## Data Types

Like other programming languages, Elixir has various data types that it supports such as: integer, float, boolean, atom, tuple, list, and many other types. 
Elixir is also dynamically typed language meaning it is not required to define data type in the code, thus when a program is ran all variables are checked during 
runtime and assigned a data typing based on their value. Additionally, the Elixir language also comes with typespecs (optional feature) which are used for declaring 
typed function signatures (or specifications) as well declaring custom types. This basically allows for a way to describe or document the expected type of a function 
and allows the programmer to keep track of how the function is intended to work, improving the readability.


## Expressions

An expression is a group or combination of values, variables, and operators that produce a result. In Elixir, expressions function the same way as other 
programming languages with a few differences. In Elixir, there are no statements that are written as everything is considered an expression, meaning 
every line will have some sort of value produced and returned. For instance, in the following expression we add two numbers together, `iex> 2 + 1` 
it would then return a value `3`. The interactive shell(iex>) allows us to use that line as a standalone expression, and the result is automatically displayed 
in the interactive shell. Like other languages an expression can be assigned to a variable like `x = 2 + 1` which then stores the result `3` inside of x.

## Assignment Statements

## Statement-Level Control Structures

**case**

Compare a given value against patterns until a match is found.

```elixir
st = "value"

case st do
    "some_other_value" -> "Won't match"
    "value" -> "Will match"
    _ -> "This will match any value.
end
```

If the `_->` catch all is omitted an error will be raised.

**cond**

Will match against different values until one is found that does not evaluate to `nil` or `false`.

```elixir
x = 4

cond do
    4 + 3 == x -> "False"
    2 * 3 == x -> "False"
    2 + 2 == x -> "True! This code is evaluated."
    true ->
end
```

In an many imperative languages this is comparable to `else if` clauses. If the `true` catch all is omitted an error will be raised.

**if** and **unless**

```elixir
if true do
    "This code evaluated"
end

unless true do
    "This will never be evaluated"
end
```

The condition provided to `if` must return true to execute what is inside the `do ... end.` block. The opposite occurs for `unless`.

## Subprograms

## Abstract Data Types and Encapsulation Concepts (if there are any)  
Elixir natively supports maps and linked lists (in fact, all lists are implemented as linked lists).  Other more complex data structures can be built from combinations of maps and lists.  In addition, Elixir allows the use of structs to group disparate data together under the same heading in a structured way.

**struct**  
The following code defines the initial template struct, then you can assign additional instances by assigning the same pattern to new data:
```elixir 
defmodule Class do
    defstruct code: "CSB-310", credits: 5, instructor: "Eric Lloyd"
end
```
then the next command produces the following output:
```elixir
%Class{code: "AD-350", instructor: "Kyle Bastien"}  

%Class{code: "AD-350", credits: 5, instructor: "Kyle Bastien"}
```
as you can see, the data that does not get remapped in the new instance carries over from the template.

## Object-Oriented Programming  
Being a functional language, Elixir does not support "objects" in the same sense as object-oriented languages such as Java.  However, it does specify "protocols" which provide some of the inheritance and polymorphism functions present in OOP.  Protocols are similar to interfaces in a language like Java, but they can be (and very often are) implemented many times within the same program to achieve different outcomes with similar data inputs.  

Even greater and more OOP-like functionality can be obtained by combining protocols and structs.  The protocol can be used to provide functionality similar to that of an object's methods, while the struct provides organized data storage to go with that functionality.  In addition, Elixir has a similar concept to generic implementations by "deriving" the protocol implementation based upon the Any data type.

**protocol**  
Protocol is used to alter behavior of a module based on the data type of the input.
```elixir
defmodule Utility do
  def type(value) when is_binary(value), do: "string"
  def type(value) when is_integer(value), do: "integer"
end
```

## Exception Handling and Event Handling

**Errors**

```sh
foo + 1
** (ArithmeticError) bad argument in arithmetic expression
```

To raise an error:

`raise <Error>, <message>`

```elixir
raise "this generic raise raises a RuntimeError"

raise ArgumentError, message: "Specific error provided and message"
```

Create your own error:

```elixir
defmodule myError do
    defexception message: "You can't do that"
end

raise MyError
```

**Rescues**

```elixir
try do
    raise "RuntimeError"
rescue
    e in RuntimeError -> e
end
```

The above would _rescue_ the RuntimeError and return the exception. Unlike some languages this construct is not often used as there are often alternative ways to handle potential problems. For example, to read in a file Elixir provides a File.read method that returns a response in the form of Ok/Error `{:ok, body}`. While one needs to write code to handle the Ok/Error it is different than handling something exceptional. In this case it is more an expected thing that may occur and need to be handled.

A common saying in the Erlang community of which Elixir is an offshoot is, _"fail fast" / "let it crash"_, which means that if something unexpected happens one should let it without rescuing it.

**Throws**

`throw` and `catch` are not commonly used. They are reserved for situations where it is not possible to get a value otherwise. Generally, when interfacing with a module that doesn't not provide and API.

```elixir
try do
    Enum.each(-50..50, fn x ->
        if rem(x, 13) == 0, do: throw(x)
    end)
    "Nothing comes out of this"
catch
    x -> "Got #{x}"
end
"Got -39" 
```

To be clear Enum does have an API so this is not necessary.

**Exits**

Elixir code runs inside processes that communicate with each other. If a process dies under normal operating procedure it sends an `exit` signal. This can also be sent explicitly.

**After**

`after` will be executed no matter if the try block succeeds or fails. Treated as a final "cleanup" of anything that might have been done during either the attempt or failure of a block of code.

```elixir
try do
    # execute some code
    raise "something"
after
    # do some cleanup
end
```

This is not guaranteed to run if a process exits.

**Else**

This will match after a try block whenever the block completes without a throw or an error occurring. However, Exceptions inside the `else` block are not caught in the overall `try/catch`.

## Example Program

```elixir
defmodule LOGPARSE do
  def get_log(file_path) do
    case File.read(file_path) do
      {:ok, body} -> String.split(body, ~r{(\r\n|\r|\n)})
      {:error, message} -> IO.puts(message)
    end
  end

  def get_by_type(text) do
    cond do
      String.contains?(text, "INFO") -> text
      String.contains?(text, "ERROR") -> text
      String.contains?(text, "WARNING") -> text
      true -> ""
    end
  end
end

defmodule Logfile do
    defstruct file: "./log.txt"
end

log = LOGPARSE.get_log(%Logfile.file)
relevant_lines = Enum.map(log, fn line -> LOGPARSE.get_by_type(line) end)
cleaned = Enum.filter(relevant_lines, fn empty -> empty != "" end)
errors = Enum.filter(cleaned, fn error -> String.contains?(error, "ERROR") end)

if length(errors) == 0 do
  IO.puts("Yay! No errors!")
end
```

## Sources

[1] "Introduction," elixir-lang.github.com. https://elixir-lang.org/getting-started/ (accessed May 31, 2023).

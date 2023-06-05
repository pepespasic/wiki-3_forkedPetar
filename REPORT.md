# The Elixer Programming Language

## Data Types

## Expressions

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

## Object-Oriented Programming

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

log = LOGPARSE.get_log("./log.txt")
relevant_lines = Enum.map(log, fn line -> LOGPARSE.get_by_type(line) end)
cleaned = Enum.filter(relevant_lines, fn empty -> empty != "" end)
errors = Enum.filter(cleaned, fn error -> String.contains?(error, "ERROR") end)

if length(errors) == 0 do
  IO.puts("Yay! No errors!")
end
```

## Sources

[1] "Introduction," elixir-lang.github.com. https://elixir-lang.org/getting-started/ (accessed May 31, 2023).
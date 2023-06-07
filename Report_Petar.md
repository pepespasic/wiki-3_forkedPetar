Functional versus Object Oriented
Values, not references
As a functional programming language, Elixir doesn’t have a concept of classes, objects or instances.


var myObj = { val: 100 };
}
console.log(myObj.val); // Output: 100

Bottom line: In Elixir, a world without references, we always pass by value.

Fault tolerance



Modules and functions
Modules 101: Basics


defmodule Some.Module.Name
  (...)
end


Functions 101: Basics

defmodule HelloWorld do
  def hello do
    IO.puts "Hello world"
  end
end

Let’s expand this example and modify our function to take some input:

def hello(subject) do
Nothing too fancy here. We can test this in the interactive shell:

$ iex -S mix
Compiling 1 file (.ex)
Interactive Elixir (1.7.2) - press Ctrl+C to exit (type h() ENTER for help)
iex(1)> HelloWorld.hello("test")
Hello test
:ok
iex(2)> HelloWorld.hello()
** (UndefinedFunctionError) function HelloWorld.hello/0 is undefined or private. Did you mean one of:
* hello/1
(hello_world) HelloWorld.hello()


So — it’s extra important to make all your functions small, simple units.

Anonymous functions

iex(2)> sayhi
#Function<20.127694169/0 in :erl_eval.expr/5>
iex(3)> sayhi()
** (CompileError) iex:3: undefined function sayhi/0
:ok

Assignment statements:
a = 0
a = if true do
Functions and Modules:
Functional versus Object Oriented
Values, not references
Let’s think about variables. In object-oriented programming, a variable refers to a memory address, which has a set value. We can usually pass that variable around (“by reference”), so that other functions can access and modify it.

Modify a variable’s value in one function, and that changes it in every other context that holds the same reference. Here’s a crude example of this in JS:

var myObj = { val: 100 };
function calc(obj) {
  obj.val *= 2; // Example: set obj.val to current value times two
}
console.log(myObj.val); // Output: 100
calc(myObj);
console.log(myObj.val); // Output: 200
Such a concept does not exist in Elixir. All data is immutable. When you assign a variable, you actually “bind” a value (that will never change) to a memory address. You can then pass that value around, but without reference.

Bottom line: In Elixir, a world without references, we always pass by value.

Fault tolerance
I want you to get excited about this new world without references. While it changes our approach to writing code a bit, it is very elegant in its own way.

A function in Elixir — one without references — does not depend on state, and cannot be affected by it. It takes input, processes it, and then returns some output. That makes functions extremely predictable and testable.

Overall, this helps with our stability, fault tolerance, and makes debugging a lot easier. Plus, it takes away the pain of threading / concurrency issues as you will never have to worry about “protecting” your state.

Modules and functions
Modules 101: Basics
Instead of classes, we have modules in Elixir. Consider modules a collection of functions. Each is available to your whole application, and its functions can be called from anywhere. It’s all “public”, “static” and stateless
Modules start with the defmodule keyword, followed by the module name. The module block ends with the end keyword. No curly braces here.

Sources:
https://elixir-lang.org/getting-started/modules-and-functions.html
https://medium.com/@roydejong/elixir-a-primer-for-object-oriented-programmers-fd5ef0206943#:~:text=Functional%20versus%20Object%20Oriented&text=As%20a%20functional%20programming%20language,which%20has%20a%20set%20value.

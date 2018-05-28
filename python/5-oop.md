# Python - Classes

## Introduction
  - It is a mixture of the class mechanisms found in C++ and Modula-3.
  - Python classes provide all the standard features of Object Oriented Programming.
  - Built-in types can be used as base classes for extension by the user.
  - Built-in operators with special syntax (arithmetic operators, subscripting etc.) can be redefined for class instances.

## Names and Objects
  - Multiple names can be bound to the same object (known as aliases).
  - Make passing object is cheaper since only the pointer is passed.

## Scopes and Namespaces

### Namespaces
A namespace is a mapping from names to objects. Most namespaces are currently implemented as Python dictionaries, but that’s normally not noticeable in any way (except for performance), and it may change in the future.
Examples:
  - the set of built-in names
  - built-in exception names
  - the global names in a module
  - the local names in a function invocation
  - the set of attributes of an object

#### Notes
- Attribute is used to refer to any name following a dot.
- Attributes may be read-only or writable.
- Writable attributes may also be deleted with the del statement.
- Namespaces are created at different moments and have different lifetimes.
- The local namespace for a function is created when the function is called, and deleted when the function returns or raises an exception that is not handled within the function.

### Scopes
- A scope is a textual region of a Python program where a namespace is directly accessible. “Directly accessible” here means that an unqualified reference to a name attempts to find the name in the namespace.
- Scopes are determined statically, they are used dynamically.
- At any time during execution, there are at least three nested scopes whose namespaces are directly accessible:
  - the innermost scope, which is searched first, contains the local names
  - the scopes of any enclosing functions, which are searched starting with the nearest enclosing scope, contains non-local, but also non-global names
  - the next-to-last scope contains the current module’s global names
  - the outermost scope (searched last) is the namespace containing built-in names
- If a name is declared global, then all references and assignments go directly to the middle scope containing the module’s global names. To rebind variables found outside of the innermost scope, the nonlocal statement can be used; if not declared nonlocal, those variables are read-only (an attempt to write to such a variable will simply create a new local variable in the innermost scope, leaving the identically named outer variable unchanged).

- Usually, the local scope references the local names of the (textually) current function. Outside functions, the local scope references the same namespace as the global scope: the module’s namespace. Class definitions place yet another namespace in the local scope.

- It is important to realize that scopes are determined textually: the global scope of a function defined in a module is that module’s namespace, no matter from where or by what alias the function is called. On the other hand, the actual search for names is done dynamically, at run time — however, the language definition is evolving towards static name resolution, at “compile” time, so don’t rely on dynamic name resolution! (In fact, local variables are already determined statically.)

- A special quirk of Python is that – if no global statement is in effect – assignments to names always go into the innermost scope. Assignments do not copy data — they just bind names to objects. The same is true for deletions: the statement del x removes the binding of x from the namespace referenced by the local scope. In fact, all operations that introduce new names use the local scope: in particular, import statements and function definitions bind the module or function name in the local scope.

- The `global` statement indicate that particular variables `live in the global scope` and should `be rebound` there.
- The `nonlocal` statement indicates that particular variables `live in an enclosing scope` and should `be rebound` there.

### Examples
```python
def scope_test():
  def do_local():
      spam = "local spam" # re-assignment -> create new local variable spam & not effect the variable  enclosing scope.

  def do_nonlocal():
      nonlocal spam # bind the spam variable in the enclosing scope.
      spam = "nonlocal spam" # change takes effect on the variable.

  def do_global():
      global spam # bind the spam variable in the global scope (has not been defined yet).
      spam = "global spam" # change takes effect.

  spam = "test spam" # variable in enclosing scope.
  do_local()
  print("After local assignment:", spam)
  do_nonlocal()
  print("After nonlocal assignment:", spam)
  do_global()
  print("After global assignment:", spam)

scope_test()
print("In global scope:", spam) # this spam variable is in global scope.
```
Result:
> After local assignment: test spam  
> After nonlocal assignment: nonlocal spam  
> After global assignment: nonlocal spam  
> In global scope: global spam  

## Classes
```python
class ClassName:
  ...
  ...
```
- must be executed before they have any effect.
- can place a class definition in a branch of an if statement, or inside a function.
- the statements inside a class definition will usually be function definitions, but other statements are allowed.
- a new namespace is created, and used as the local scope.
- `__init__()` is a special method that works as constructor.
```python
class Complex:
  def __init__(self, realpart, imagpart):
    self.r = realpart
    self.i = imagpart

x = Complex(3.0, -4.5)
x.r, x.i # => (3.0, -4.5)
```
- there are two kinds of valid attribute names, data attributes and methods.
- object methods and class functions.
```python
class MyClass:
  def __init__(self, name):
    self.name = name

  def speak(self, something):
    print(self.name + ' says: ' + something)

ken = MyCalss('Ken')
ken.speak('Hello') # this equally to MyClass.speak(ken) => 'Ken says: Hello'
```
- all variables define in class without `self` keyword is `class variables` other is `instance variables`.
- inheritance is support by below syntax:
```python
class DerivedClassName(BaseClassName):
  <statement-1>
  .
  .
  .
  <statement-N>

# or multiple inheritances
class DerivedClassName(Base1, Base2, Base3):
  <statement-1>
  .
  .
  .
  <statement-N>
```
- simple way to call the base class method: `BaseClassName.methodname(self, arguments)`.
- two built-in functions that work with inheritance:
  - `isinstance()` to check an instance’s type.
  - `issubclass()` to check class inheritance.
- private variable doesn't exist in Python.
- it is convention that variables with `_` prefix should be treated as non-public part.

## Iterators
Define an `__iter__()` method which returns an object with a `__next__()` method. If the class defines `__next__()`, then `__iter__()` can just return self:
```python
class Reverse:
  """Iterator for looping over a sequence backwards."""
  def __init__(self, data):
    self.data = data
    self.index = len(data)

  def __iter__(self):
    return self

  def __next__(self):
    if self.index == 0:
        raise StopIteration
    self.index = self.index - 1
    return self.data[self.index]
```

## Generators
Generators are a simple and powerful tool for creating iterators. The `__iter__()` and `__next__()` methods are created automatically.
```python
def reverse(data):
    for index in range(len(data)-1, -1, -1):
        yield data[index]
```
Generator Expressions



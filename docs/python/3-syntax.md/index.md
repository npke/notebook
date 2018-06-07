# Python - Syntax

## Control flow
### If 
```python
if condition:
  ...
elif conditon:
  ...
else:
```
  - can contain one or many `elif` parts.
  - `else` is optinal.

### For
  - iterate through items of any sequence (list or string), in the order that they appear in the sequence.
```python
animals = ['cow', 'pig', 'parrot', 'owl']
for ani in animals:
  print(ani)
```
  - use `range(start, end, step)` to iterate over a sequence of numbers.
  - the object returned by range() behaves as if it is a list, but in fact it isn’t. It is an object which returns the successive items of the desired sequence when you iterate over it, but it doesn’t really make the list, thus saving space.
```python
range(5, 10) # => 5, 6, 7, 8, 9

range(0, 10, 3) # => 0, 3, 6, 9

range(-10, -100, -30) # => -10, -40, -70
```
  - `list()` create a list from iterables.
  - loop statements may have an else clause; it is executed when the loop terminates through exhaustion of the list (with `for`) or when the condition becomes false (with `while`), but not when the loop is terminated by a break statement.


### `Pass` statement
  - the `pass` statement does nothing.
  - commonly used for creating minimal classes.
```python
class MyEmptyClass:
  pass
```
  - can be used is as a place-holder for a function or conditional body.
```python
def initlog(*args):
  pass   # Remember to implement this!
```

### Functions
  - defined with the keyword `def` follow by function name and parameters inside a parentheses.
  - usually contain a `docstring` line for description & documentation purpose.
  - variable assignment inside a function will store the value in a local symbol table.
  - variable references first look in the local symbol table -> enclosing function -> global -> built-in.
  - variables (include argument, outside variable) are referenced but can not be re-assigned.
  - if no `return` statement is specified or return without a value then `None` is returned.

  ```python
  def fibonaci(n):    # write Fibonacci series up to n
    """Print a Fibonacci series up to n."""
    a, b = 0, 1
    while a < n:
        print(a, end=' ')
        a, b = b, a+b
    print()
  ```

  ```python
  def isPrime(n):
    """Check if a number is prime or not"""
    if n <= 1: return False
    if n == 2: return True

    for i in range(2, n):
      if n % i == 0: return False
    else: return True
  ```
#### Default parameters
  - assign value to a parameter to set default value for that parameter.
  ```python
  def functionName(paremeter1 = value1, parameter2 = value2):
  ...
  ```
  - the function then can be called with fewer arguments.
  - default value only evaluate once and mutable.
  ```python
  def f(a, L=[]):
    L.append(a)
    return L

  print(f(1)) # => [1]
  print(f(2)) # => [1, 2]
  print(f(3)) # => [1, 2, 3]
  ```

#### Keyword arguments
  - function with default arguments can be called in form of `key=value`
  - if `*name1` and `**name2` is put as final formal parementer then `name1` will be a tuple with passed argument and `name2` will be a dictionary with key, value of arguments.
  - function call with the `*` operator will unpack the arguments out of a list or tuple or `**` operator for dictionary.

#### Lamda expresstions
  - used to create anonymous function.
  - return just one value in the form of an expression.
  - cannot access variables other than those in their parameter list and those in the global namespace.
  ```python
  # declare an anonymous function that sum up two numbers and return
  lambda a, b: a + b
  ```

#### Coding styles
  - use 4 spaces indent.
  - source code in UTF-8 encoding.
  - CamelCase for class names and snake_case for methods and functions.



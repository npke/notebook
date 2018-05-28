# Python - Data Types

## Indentifiers
  - must start with a letter or the underscore character.
  - cannot start with a number.
  - can only contain alpha-numeric characters and underscores (A-z, 0-9, and _ ).
  - variable names are case-sensitive.

## Numbers
  - integers (2, 4, 20) have type `int`.
  - numbers with fractional part have type `float`.
  - there are more numberic types (`Decimal`, `Fraction`, `complex numbers`).
  - divison (`/`) always return float.
  - use double slashes (`//`) to make a floor divison.
  - use double stars (`**`) to calculate powers.

## Strings
  - enclosed in single quote or double quote.
  - use `\` to escape quote.
  - characters with `\` prefix will not be interpreted as special character if put `r` before the opening quote.
  - to span mutiple line use `docstrings`: triple quotes `'''...'''` or `"""..."""`.
  - concatenate strings by `+` operator.
  - repeat string by `*` operator.
  - string literals will be concatenated automatically.
  - string indices start from 0, negative value for starting from the end.
  - slice string by `string[start:end]`.
  - `string[:end]` -> start = 0;
  - `string[start:]` -> end = the last character index.
  - strings are immutable.
  - built-in function `len(string)` return the length of a string.

## Lists
  - a list of comma-separated values inside square brackets.
  - can contain different types.
  - can be indexed and sliced.
  - slice operation return a new list (shadow copy).
  - lists are mutable.
  - add new element by using `list.append()`.
  - replace a range by re-assign `list[start:end] = []`.
  - remove by re-assign the range with an emty list.
  - common used methods: `append(x)`, `extend(iterable)`, `insert(i, x)`, `remove(x)`, `pop([i])`,
  `clear()`, `index(x[, start[, end]])`, `count(x)`, `sort(key=None, reverse=False)`, `reverse()`, `copy()`.
  - delete element by index.
  ```python
  a = [-1, 1, 66.25, 333, 333, 1234.5]
  del a[0]
  a # => [1, 66.25, 333, 333, 1234.5]
  del a[2:4]
  a # => [1, 66.25, 1234.5]
  del a[:]
  a # => []
  ```
  - del can also be used to delete entire variables. `del a`

### List comprehensions
  - provide a consise way to create lists.
  ```python
  squares = [x**2 for x in range(10)] # => [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

  [(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]
  # =>[(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]
  ```

## Tuples
  - are immutable and contain a heterogeneous sequence of elements (the element can be mutable).
  - are accessed via unpacking or indexing.
  - consists of a number of values separated by commas (can be constructed in reverse form).
  - empty tuples are constructed by an empty pair of parentheses.
  - a tuple with one item is constructed by following a value with a comma.
  ```python
  empty = ()
  singleton = 'hello',    # <-- note trailing comma

  len(empty) # => 0
  len(singleton) # => 1

  singleton # => ('hello',)
  ```

## Sets
  - unordered collection with no duplicate elements.
  - support mathematical operations like union, intersection, difference, and symmetric difference.
  - are constructed by putting elements inside a pair of curly brackets.
  - check if the element is in the sets by `in` keyword `'orange' in fruits`.
  - set comprehensions are also supported.
  ```python
  fruits = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}
  if 'orange' in fruits: # => True
  ```

## Dictionaries
  - a key - value set.
  - key can be any of immutable types: strings, numbers, tuples (only contains immutable elements).
  - a pair of curly brackets `{}` create an empty dictionary.
  - key - value are comma-separated in a dictionary.
  - use `del` to delete a key - value
  ```python
  tel = {'jack': 4098, 'sape': 4139}
  tel['guido'] = 4127

  tel # => {'sape': 4139, 'guido': 4127, 'jack': 4098}

  tel['jack'] # => 4098

  del tel['sape']
  tel['irv'] = 4127

  tel # => {'guido': 4127, 'irv': 4127, 'jack': 4098}
  list(tel.keys()) # => ['irv', 'guido', 'jack']
  sorted(tel.keys()) # => ['guido', 'irv', 'jack']
  'guido' in tel # => True
  'jack' not in tel # => False
  ```
  - `dict()` constructor builds dictionaries directly from sequences of key-value pairs.
  ```python
  dict([('sape', 4139), ('guido', 4127), ('jack', 4098)]) 
  # => {'sape': 4139, 'jack': 4098, 'guido': 4127}
  ```
  - dict comprehensions can be used to create dictionaries from arbitrary key and value expressions.
  ```python
  { x: x**2 for x in (2, 4, 6) }
  # => {2: 4, 4: 16, 6: 36}
  ```
  - when the keys are simple strings, it is sometimes easier to specify pairs using keyword arguments:
  ```python
  dict(sape=4139, guido=4127, jack=4098)
  # => {'sape': 4139, 'jack': 4098, 'guido': 4127}
  ```
## Looping techniques
  - dictionaries, the key and corresponding value can be retrieved at the same time using the items() method.
  ```python
  knights = {'gallahad': 'the pure', 'robin': 'the brave'}
  for k, v in knights.items():
    print(k, v)

  # gallahad the pure
  # robin the brave
  ```
  - sequence, the position index and corresponding value can be retrieved at the same time using the enumerate() function.
  ```python
  for i, v in enumerate(['tic', 'tac', 'toe']):
    print(i, v)
  # 0 tic
  # 1 tac
  # 2 toe
  ```
  - loop over two or more sequences at the same time, the entries can be paired with the zip() function.
  ```python
  questions = ['name', 'quest', 'favorite color']
  answers = ['lancelot', 'the holy grail', 'blue']
  for q, a in zip(questions, answers):
    print('What is your {0}?  It is {1}.'.format(q, a))

  # What is your name?  It is lancelot.
  # What is your quest?  It is the holy grail.
  # What is your favorite color?  It is blue.
  ```

  - loop over a sequence in reverse, first specify the sequence in a forward direction and then call the reversed() function.
  ```python
  for i in reversed(range(1, 10, 2)):
    print(i)

  # 9
  # 7 
  # 5 
  # 3
  # 1
  ```
  - loop over a sequence in sorted order, use the sorted() function which returns a new sorted list while leaving the source unaltered.
  ```python
  basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
  for f in sorted(set(basket)):
    print(f)

  # apple
  # banana
  # orange
  # pear
  ```

## Conditions
  - `in` and `not in` check whether a value occurs (does not occur) in a sequence.
  - `is` and `is not` compare whether two objects are really the same object; this only matters for mutable objects like lists.
  - comparison operators have the same priority, which is lower than that of all numerical operators.
  - comparisons can be chained. 
  ```python
  a < b == c # => tests whether a is less than b and moreover b equals c.
  ```
  - boolean operators are: `and`, `or`, `not`, lower priorities than comparison operators, not has the highest priority and or the lowest.
  - sequence objects may be compared to other objects with the same sequence type, using the lexicographical order.



## Others
  - multi assigment `a, b = 1, 2`
  - while loop body is indented.






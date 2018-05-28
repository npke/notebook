# Python - Input & Ouput

## 3 ways to write output
  - `print()`
  - expression statements
  - file's `write()` method.

## Formatting
  - `str()` => representations of values which are fairly human-readable.
  - `repr()` => representations which can be read by the interpreter.
  ```python
  gretting = 'Hello, World\n'
  str(gretting) # => 'Hello, World
                #'
  repr(gretting) # => 'Hello, World\n'
  ```
  - `string.rjust()`, ``string.ljust()`, string.center()` return string with specified width, padding with spaces to left or right.
  - `string.zfill()` pads a numeric string on the left with zeros.
  - `string.format`
    ```python
    print('We are the {} who say "{}!"'.format('knights', 'Ni'))
    ```
    - positional arguments and keyword arguments can be used.
    ```python
    print('{0} and {1}'.format('spam', 'eggs'))
    print('This {food} is {adjective}.'.format(food='spam', adjective='absolutely horrible'))
    ```
    - '!a' (apply `ascii()`), '!s' (apply `str()`) and '!r' (apply `repr()`) can be used to convert the value before it is formatted.
    - an optional ':' and format specifier can follow the field name.
    - the % operator can also be used for string formatting.
    ```python
    import math
    print('The value of PI is approximately %5.3f.' % math.pi)
    ```

## Keyboard input
  - `input(prompt)` - read input from keyboard.


## File read
  - open a file with `open(filename, mode)`
    - `r`: only read (default).
    - `w`: only write.
    - `r+`: both read and write.
    - `a`: write to the end.
  - files are opened in text mode by default, append `b` the `mode` argument to open in binary mode.
  - use with keyword when opening a file => file is properly closed (shorter than try-finally).
  ```python
  with open('filename', 'r+') as file:
    data = file.read()
  file.closed() # => True
  ```
  - `file.read(n)` read n bytes from file.
  - `file.readline()` read single line from file.
  - use `file.readlines()` or `list(f)` to read all lines.

## File writing
  - `file.write(str)` writes a string to file.
  - `file.tell()` return current position in file.
  - `file.seek(offet, start)` move the cursor to another position.

## Working with JSON file
  - use `json` module
  - `json.dumps(data)` => return JSON representation of data.
  - `json.dump(data, file)` => write data JSON format to file.
  - `json.load(file)` => read and serialize the JSON data in the file.
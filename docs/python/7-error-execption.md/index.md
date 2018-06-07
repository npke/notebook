# Python - Errors & Exceptions

## Error types
  - syntax error: error in script's syxtax.
  - exceptions: error when executing the script.

## Exceptions handling
  - use `try - except - else - finally`
  ```python
  import sys

  try:
      f = open('myfile.txt')
      s = f.readline()
      i = int(s.strip())
  except OSError as err:
      print("OS error: {0}".format(err))
  except ValueError:
      print("Could not convert data to an integer.")
  except:
      print("Unexpected error:", sys.exc_info()[0])
      raise
  ```
  - raise an exeception by the `raise` keyword `raise NameError('HiThere')`.

## User-defined exceptions
  - should typically be derived from the Exception class.
  - exceptions are defined with names that end in `Error`.
  ```python
  class Error(Exception):
    """Base class for exceptions in this module."""
    pass

  class InputError(Error):
    """Exception raised for errors in the input.

    Attributes:
        expression -- input expression in which the error occurred
        message -- explanation of the error
    """

    def __init__(self, expression, message):
        self.expression = expression
        self.message = message
  ```

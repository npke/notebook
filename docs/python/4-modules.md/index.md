# Python - Modules

## Introduction
  - inside a module, the module's name is available as the global variable `__name__`
  - each module has its own private symbol table.
  - there are variants of module import statements:
  ```python
  from fibo import fib, fib2
  fib(500) # => 1 1 2 3 5 8 13 21 34 55 89 144 233 377

  # imports all names except those beginning with an underscore (_)
  from fibo import *
  fib(500) # => 1 1 2 3 5 8 13 21 34 55 89 144 233 377

  # rename when importing
  import fibo as fib

  from fibo import fib as fibonacci
  ```
  - when run a module by Python interpreter via commandline, the `__name__` variable will be set as `__main__`.

## Module search path 
  - first searches for a built-in module with that name.
  - if not found, it then searches for file with module name in a list of directories given by the variable `sys.path`.
  - `sys.path` is initialized from these locations:
    - The directory containing the input script (or the current directory when no file is specified).
    - `PYTHONPATH` (a list of directory names, with the same syntax as the shell variable `PATH`).
    - The installation-dependent default.
  - after initialization, Python programs can modify `sys.path`.

## Compiled Python files
To speed up loading modules, Python caches the compiled version of each module in the `__pycache__` directory under the name module.version.pyc, where the version encodes the format of the compiled file; it generally contains the Python version number.  

Python checks the modification date of the source against the compiled version to see if it’s out of date and needs to be recompiled.

Python does not check the cache in two circumstances.
  - First, it always recompiles and does not store the result for the module that’s loaded directly from the command line.
  - Second, it does not check the cache if there is no source module. To support a non-source (compiled only)  distribution, the compiled module must be in the source directory, and there must not be a source module.

A program doesn’t run any faster when it is read from a .pyc file than when it is read from a .py file; the only thing that’s faster about .pyc files is the speed with which they are loaded.

## The dir() Function
The built-in function dir() is used to find out which names a module defines. It returns a sorted list of strings. Without arguments, dir() lists the names you have defined currently.
```python
import fibo, sys
dir(fibo) # => ['__name__', 'fib', 'fib2']

dir() # => ['__builtins__', '__name__', 'a', 'fib', 'fibo', 'sys']
```

## Packages
- Packages are a way of structuring Python’s module namespace by using “dotted module names”.
```
sound/                          Top-level package
      __init__.py               Initialize the sound package
      formats/                  Subpackage for file format conversions
              __init__.py
              wavread.py
              wavwrite.py
              aiffread.py
              aiffwrite.py
              auread.py
              auwrite.py
              ...
      effects/                  Subpackage for sound effects
              __init__.py
              echo.py
              surround.py
              reverse.py
              ...
      filters/                  Subpackage for filters
              __init__.py
              equalizer.py
              vocoder.py
              karaoke.py
              ...
```

- When importing the package, Python searches through the directories on sys.path looking for the package subdirectory.  

- The `__init__.py` files are required to make Python treat the directories as containing packages. It can also execute initialization code for the package or set the `__all__` variable - it is taken to be the list of module names that should be imported when `from package import *` is encountered.
```python
__all__ = ["echo", "surround", "reverse"]
```

- When using `from package import item`, the `item` can be either a submodule (or subpackage) of the package, or some other name defined in the package, like a function, class or variable. The `import` statement first tests whether the item is defined in the package; if not, it assumes it is a module and attempts to load it.

- When using syntax like `import item.subitem.subsubitem`, each item except for the last must be a package; the last item can be a module or a package but can’t be a class or function or variable defined in the previous item.

## Intra-package References
> pass

## Packages in Multiple Directories
> pass
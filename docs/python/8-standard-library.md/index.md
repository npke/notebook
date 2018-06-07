# Python - The Standard Library

## Operating System Interface
- The `os` module provides dozens of functions for interacting with the operating system.
- The built-in `dir()` and `help()` functions are useful as interactive aids for working with large modules like os.
- For daily file and directory management tasks, the `shutil` module provides a higher level interface that is easier to use.
- Use the `import os` style instead of `from os import *`.

## String Pattern Matching
- The `re` module provides regular expression tools for advanced string processing.

## Mathematics
- The `math` module gives access to the underlying C library functions for floating point math.
- The `random` module provides tools for making random selections.
- The `statistics` module calculates basic statistical properties (the mean, median, variance, etc.) of numeric data.
- The SciPy project (https://scipy.org)[https://scipy.org] has many other modules for numerical computations.

## Internet Access
- There are a number of modules for accessing the internet and processing internet protocols. Two of the simplest are `urllib.request` for retrieving data from URLs and `smtplib` for sending mail.

## Dates and Times
- The `datetime` module supplies classes for manipulating dates and times in both simple and complex ways.

## Data Compression
- Common data archiving and compression formats are directly supported by modules including: `zlib`, `gzip`, `bz2`, `lzma`, `zipfile` and `tarfile`.

## Quality Control
- The `doctest` module provides a tool for scanning a module and validating tests embedded in a program’s docstrings.
- The `unittest` module is not as effortless as the doctest module, but it allows a more comprehensive set of tests to be maintained in a separate file.

## Multi-threading
- `threading` module can run tasks in background while the main program continues to run

## Logging
- The `logging` module offers a full featured and flexible logging system.
```python
import logging

logging.debug('Debugging information')
logging.info('Informational message')
logging.warning('Warning:config file %s not found', 'server.conf')
logging.error('Error occurred')
logging.critical('Critical error -- shutting down')
```

## Tools for Working with Lists
- The `array` module provides an array() object that is like a list that stores only homogeneous data and stores it more compactly.
- The `collections` module provides a deque() object that is like a list with faster appends and pops from the left side but slower lookups in the middle. These objects are well suited for implementing queues and breadth first tree searches.
- The `heapq` module provides functions for implementing heaps based on regular lists. The lowest valued entry is always kept at position zero. This is useful for applications which repeatedly access the smallest element but do not want to run a full list sort.

## Decimal Floating Point Arithmetic
The `decimal` module offers a `Decimal` datatype for `decimal` floating point arithmetic. Compared to the built-in `float` implementation of binary floating point, the class is especially helpful for

- financial applications and other uses which require exact decimal representation,
- control over precision,
- control over rounding to meet legal or regulatory requirements,
- tracking of significant decimal places, or applications where the user expects the results to match calculations done by hand.
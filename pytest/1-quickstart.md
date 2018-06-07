# PyTest - Quickstart

## Installation
```bash
pip install pytest
```

## First test
```python
def func(x):
    return x + 1

def test_answer():
    assert func(3) == 5
```
- run tests by `pytest` command
- pytest will run all files of the form `test_*.py` or `*_test.py` in the current directory and its subdirectories
- group multiple tests into a class
```python
class TestClass(object):
    def test_one(self):
        x = "this"
        assert 'h' in x

    def test_two(self):
        x = "hello"
        assert hasattr(x, 'check')
```

## Exit codes
- `0` All tests were collected and passed successfully
- `1` Tests were collected and run but some of the tests failed
- `2` Test execution was interrupted by the user
- `3` Internal error happened while executing tests
- `4` pytest command line usage error
- `5` No tests were collected

## Running tests
- stop the testing after failers
```bash
pytest -x            # stop after first failure
pytest --maxfail=2    # stop after two failures
```

- run tests in a module
```bash
pytest test_mod.py
```
- run tests in a directory
```bash
pytest testing/
```

- run tests by keyword expressions
```bash
pytest -k "MyClass and not method"
```

- get a list of the slowest 10 test durations
```bash
pytest --durations=10
```

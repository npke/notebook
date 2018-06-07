# PyTest - Assertions

## `assert` statement
- use the standard python `assert` for verifying expectations and values 
```python
# content of test_assert1.py
def f():
    return 3

def test_function():
    assert f() == 4

assert a % 2 == 0, "value was odd, should be even"
```

## Assertions about expected exceptions
- use pytest.raises as a context manager
```python
import pytest

def test_zero_division():
    with pytest.raises(ZeroDivisionError):
        1 / 0
```
- specify a “raises” argument to `pytest.mark.xfail`
```python
@pytest.mark.xfail(raises=IndexError)
def test_f():
    f()
```

## Defining a assertion comparison
> pass

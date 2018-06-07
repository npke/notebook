# PyTest - Marking test functions with attributes

By using the pytest.mark helper you can easily set metadata on your test functions.
- `skip` - always skip a test function
- `skipif` - skip a test function if a certain condition is met
- `xfail` - produce an “expected failure” outcome if a certain condition is met
- `parametrize` to perform multiple calls to the same test function.

## Skipping test functions
A `skip` means that you expect your test to pass only if some conditions are met, otherwise pytest should skip running the test altogether.

A `xfail` means that you expect a test to fail for some reason.

- pytest counts and lists skip and xfail tests separately.

### Usage
- mark it with the `skip` decorator which may be passed an optional `reason`.
- call the `pytest.skip(reason)` function.
- to skip the whole module using `pytest.skip(reason, allow_module_level=True)` at the module level.
```python
@pytest.mark.skip(reason="no way of currently testing this")
def test_the_unknown():
    ...

def test_function():
    if not valid_config():
        pytest.skip("unsupported configuration")

if not pytest.config.getoption("--custom-flag"):
    pytest.skip("--custom-flag is missing, skipping tests", allow_module_level=True)
```
- skip something conditionally then use `skipif` instead.
```python
import sys
@pytest.mark.skipif(sys.version_info < (3,6),
                    reason="requires python3.6")
def test_function():
    ...

import mymodule
minversion = pytest.mark.skipif(mymodule.__versioninfo__ < (1,1),
                                reason="at least mymodule-1.1 required")
@minversion
def test_function():
    ...

# import the marker and reuse it in another test module
from test_mymodule import minversion

@minversion
def test_anotherfunction():
    ...
```

## XFail: mark test functions as expected to fail
- use the `xfail` marker to indicate that expected to be failed.
```python
@pytest.mark.xfail
def test_function():
    ...

def test_function():
    if not valid_config():
        pytest.xfail("failing configuration (but should work)")
```

### Skip/xfail with parametrize
```python
import pytest

@pytest.mark.parametrize(
    ("n", "expected"),
    [
        (1, 2),
        pytest.param(1, 0, marks=pytest.mark.xfail),
        pytest.param(1, 3, marks=pytest.mark.xfail(reason="some bug")),
        (2, 3),
        (3, 4),
        (4, 5),
        pytest.param(
            10, 11, marks=pytest.mark.skipif(sys.version_info >= (3, 0), reason="py2k")
        ),
    ],
)
def test_increment(n, expected):
    assert n + 1 == expected
```

## Parametrizing fixtures and test functions
Enabled at several levels:
- `pytest.fixture()` allows one to parametrize fixture functions.
- `@pytest.mark.parametrize` allows one to define multiple sets of arguments and fixtures at the test function or class.
- `pytest_generate_tests` allows one to define custom parametrization schemes or extensions.

### `@pytest.mark.parametrize`: parametrizing test functions
```python
import pytest

@pytest.mark.parametrize("test_input,expected", [
    ("3+5", 8),
    ("2+4", 6),
    ("6*9", 42),
])
def test_eval(test_input, expected):
    assert eval(test_input) == expected
```

### `pytest_generate_tests`
Implement a parametrization scheme or implement some dynamism for determining the parameters or scope of a fixture.
> pass
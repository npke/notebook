# PyTest - Fixtures

## Introduction
- provide pre-defined operations that can be used/executed later by test functions.
- have explicit names, are activated by declaring their use from test functions, modules, classes or whole projects.
- implemented in a modular manner, a fixture can itself uses other fixtures.
- management scales from simple unit to complex functional testing.

> Fixtures allow test functions to easily receive and work against specific pre-initialized application objects without having to care about import/setup/cleanup details. It’s a prime example of dependency injection where fixture functions take the role of the injector and test functions are the consumers of fixture objects.

## Usage
- fixture functions are registered by marking them with `@pytest.fixture`.
```python
import pytest

@pytest.fixture
def smtp():
    import smtplib
    return smtplib.SMTP("smtp.gmail.com", 587, timeout=5)
```
- test functions can receive fixture objects by naming them as an input argument.
```python
def test_ehlo(smtp):
    response, msg = smtp.ehlo()
    assert response == 250
    assert 0 # for demo purposes
```
- can accept the `request` object to introspect the “requesting” test function, class or module context.
- sharing fixture functions are put in `conftest.py`, it automatically gets discovered by pytest.
- the discovery of fixture functions starts at test classes, then test modules, then conftest.py files and finally builtin and third party plugins.
- sharing test data is put in `tests` folder or use plugins like `pytest-datadir` and `pytest-datafiles`.

## Scope
> Sharing a fixture instance across tests in a class, module or session then the fixture instance is reuse, thus save time, resource.
- module
- session
- class
```python
import pytest
import smtplib

@pytest.fixture(scope="module")
def smtp():
    return smtplib.SMTP("smtp.gmail.com", 587, timeout=5)

```
- fixture of higher-scopes (such as `session`) are instantiated first than lower-scoped fixtures (such as `function` or `class`).

## Fixture finalization / executing teardown code
- use a `yield` statement instead of return, all the code after the `yield` statement serves as the teardown code.
```python
import smtplib
import pytest


@pytest.fixture(scope="module")
def smtp():
    smtp = smtplib.SMTP("smtp.gmail.com", 587, timeout=5)
    yield smtp  # provide the fixture value
    print("teardown smtp")
    smtp.close()
```
- can also seamlessly use the `yield` syntax with with statements.
```python
import smtplib
import pytest


@pytest.fixture(scope="module")
def smtp():
    with smtplib.SMTP("smtp.gmail.com", 587, timeout=5) as smtp:
        yield smtp  # provide the fixture value
```
>  If an exception happens during the setup code (before the yield keyword), the teardown code (after the yield) will not be called.
- alternative option is to make use of the addfinalizer method of the `request-context` object to register finalization functions.
```python
import smtplib
import pytest


@pytest.fixture(scope="module")
def smtp(request):
    smtp = smtplib.SMTP("smtp.gmail.com", 587, timeout=5)

    def fin():
        print("teardown smtp")
        smtp.close()

    request.addfinalizer(fin)
    return smtp  # provide the fixture value
```

### `yield` vs `addfinalizer`
- it is possible to register multiple finalizer functions.
- `finalizers` will always be called regardless if the fixture setup code raises an exception.
> If an exception happens before the finalize function is registered then it will not be executed.

## Factories as fixtures
- help in situations where the result of a fixture is needed multiple times in a single test.
- the fixture returns a function which generates the data instead of returning data directly.
```python
@pytest.fixture
def make_customer_record():

    def _make_customer_record(name):
        return {
            "name": name,
            "orders": []
        }

    return _make_customer_record


def test_customer_records(make_customer_record):
    customer_1 = make_customer_record("Lisa")
    customer_2 = make_customer_record("Mike")
    customer_3 = make_customer_record("Meredith")
```

## Parametrizing fixtures
- Fixture functions can be parametrized in which case they will be called multiple times, each time executing the set of dependent tests.
```python
import pytest
import smtplib

@pytest.fixture(scope="module",
                params=["smtp.gmail.com", "mail.python.org"])
def smtp(request):
    smtp = smtplib.SMTP(request.param, 587, timeout=5)
    yield smtp
    print ("finalizing %s" % smtp)
    smtp.close()
```

## Overriding fixtures
> pass
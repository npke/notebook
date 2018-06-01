# Flask - Test coverage

- Use `pytest` and `coverage` to test and measure code
```python
pip install pytest coverage
```

## Setup and Fixtures
- test code is located in `tests` folder.
- `tests/conftest.py` file contains setup functions called fixtures that each test will use.
- tests are in Python modules that start with `test_`.
- test function in those modules also starts with `test_`.
- fixtures is matched by their function names with the names of arguments in the test functions.

## Tests
```python
from flaskr import create_app

def test_config():
    assert not create_app().testing
    assert create_app({'TESTING': True}).testing

def test_hello(client):
    response = client.get('/hello')
    assert response.data == b'Hello, World!'
```
- `pytest.mark.parametrize` tells Pytest to run the same test function with different arguments.
```python
@pytest.mark.parametrize(('username', 'password', 'message'), (
    ('a', 'test', b'Incorrect username.'),
    ('test', 'a', b'Incorrect password.'),
))
def test_login_validate_input(auth, username, password, message):
    response = auth.login(username, password)
    assert message in response.data
```

## Run the tests
- some test configuration can be added to the project’s `setup.cfg` file.
```python
[tool:pytest]
testpaths = tests

[coverage:run]
branch = True
source =
    flaskr
```
- run the tests use the `pytest` command.
- measure the code coverage use the `coverage` command.
```python
coverage run -m pytest
```
- generate HTML report use the `coverage html` command.

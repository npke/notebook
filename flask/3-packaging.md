# Flask - Packaging
Build a distribution file and install that in another environment. This makes deploying the project the same as installing any other library.

## Describe the project
- The `setup.py` file describes your project and the files that belong to it.
```python
from setuptools import find_packages, setup

setup(
    name='flaskr',
    version='1.0.0',
    packages=find_packages(),
    include_package_data=True,
    zip_safe=False,
    install_requires=[
        'flask',
    ],
)
```
- Another file named `MANIFEST.in` to tell what this other data is.
```python
include flaskr/schema.sql
graft flaskr/static
graft flaskr/templates
global-exclude *.pyc
```

## Install the project
- Use pip to install your project in the virtual environment.
```python
pip install -e .
```

## Build and Install
- deploy the application need a distribution file.
- current standard for Python distribution is the wheel format (`.whl`).
```python
pip install wheel
```
- he `bdist_wheel` command will build a wheel distribution file.
```python
python setup.py bdist_wheel
```
- the distribution in placed in `dist` folder.
- the distribution can be install in other machine by `pip`.
```python
pip install app-name.whl
```
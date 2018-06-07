# Python - Virtual Environments and Packages

## Virtual Environments
- `venv` used to create and manage virtual environments.
- create a virtual env `python3 -m venv project-name`
- active the env `source project-name/bin/activate`
- there are two common tools for creating Python virtual env:
  - `venv` - default in Python3.3 and later, install `pip` and `setuptools`
  - `virtualenv` - need to be installed separately, support Python2.6+, Python3.3+, install `pip`, `setuptools` and `wheel`

### pip vs setuptools vs wheel
- `pip` - tool for installing Python packages (PyPA recommended)
- `setuptools` - a collection of utils allow to more easily build and distribute Python distributions
- `wheel` - provides a `bdist_wheel` command for setuptools, wheel files can be installed with a newer pip for with wheel’s own commandline.

## Packages
- install, upgrade, and remove packages using a program called `pip.`
- by default pip will install packages from the Python Package Index.
- pip has a number of subcommands: “search”, “install”, “uninstall”, “freeze”, etc
- install the latest version of a package by specifying a package’s name `pip install PackageName`
- install a specific version of a package by giving the package name followed by == and the version number.
- `pip uninstall` followed by one or more package names will remove the packages from the virtual environment.
- `pip show` will display information about a particular package.
- `pip list` will display all of the packages installed in the virtual environment.
- `pip freeze` will produce a similar list of the installed packages, but the output uses the format that `pip install `expects.
- A common convention is to put this list in a requirements.txt file, then install all the necessary packages with `pip install -r requirements.txt`.
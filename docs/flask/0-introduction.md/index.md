# Flask - Introduction

## Python's Web Frameworks

### Django
#### Highlights
- a high-level Python web framework
- enables rapid development of secure and maintainable websites
- takes care of much of the hassle of web development
- has a thriving and active community (free & opensource)
- great documentation
- follow monolithic architecture


#### History
- initially developed between 2003 and 2005
- first milestone release (1.0) in 2008
- recently-released version 2.0 (2017)

### Flask
#### Highlights
- micro-framework, comes with basic functionality, but allows to easily expand
- began as a simple wrapper around Werkzeug and Jinja
- doesn't enforce any dependencies or project layout
- many extensions provided by the community that make adding new functionality easy

#### History
- initial release in 2010
- lastest release (1.0.2) 2018

#### Built-in features
- built-in development server and debugger
- integrated unit testing support
- RESTful request dispatching
- uses Jinja2 templating
- support for secure cookies (client side sessions)
- 100% WSGI 1.0 compliant
- Unicode based
- extensively documented

#### Installation
- using with Python3 recommended (Python3.4+)
- distributions will be installed automatically when installing Flask:
  - `Werkzeug` implements WSGI, the standard Python interface between applications and servers.
  - `Jinja` is a template language that renders the pages your application serves.
  - `MarkupSafe` comes with `Jinja`. It escapes untrusted input when rendering templates to avoid injection attacks.
  - `ItsDangerous` securely signs data to ensure its integrity. This is used to protect Flask’s session cookie.
  - `Click` is a framework for writing command line applications.

#### Create a virtual environment
```bash
mkdir project-name
cd project-name
python3 -m venv venv
```
#### Active environment
```bash
. venv/bin/activate
```
#### Install Flask
```bash
pip install Flask
```

### Others Tornado, Pyramid ...
> pass

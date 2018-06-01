# Flask - Blueprints & Views

## Blueprints
- a way to organize related routes/views and other code (like grouping endpoint for the same resource).
- ex: blueprints for `auth` (register/login/logout), for `post` (create/read/update/delete).
- blueprints is ussually put in separate modules.

### Usage
```python
from flask import Blueprint

bp = Blueprint('auth', __name__, prefix_path='/auth')
```
- a blueprint is constructed by class `Blueprint`, the prefix_path will be prepended to all routes associated with the blueprint.
- register the blueprint in Flask's app by `app.register_blueprint(bp)`

### Route & view
- register new view (route) by the decorator `@blueprint.route(url)`
```python
@bp.route('/posts')
def allPosts():
    return 'This page shows all posts'

```
### Require Authentication
- use a decorator to check the authentication status
```python
def auth_required(view):
    @functools.wrap(view)
    def wrapped_view(*args, **kwargs):
        if g.user is None:
            return redirect(url_for('auth/login'))
        return view(*args, **kwargs)
    return wrapped_view
```
- `functools.wrap` keep the name and arguments list of original function which help it easier for debugging [more](https://stackoverflow.com/questions/308999/what-does-functools-wraps-do) 
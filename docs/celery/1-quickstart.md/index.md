# Celery - Quickstart

## Installation
- install `celery` from PyPI
- install the message broker package like `redis`, `rabbitmq`

```bash
pip install celery
pip install redis
```

## App
- instantiate a Celery app by `Celery` class.
- `broker`, `backend` are keyword arguments that used to config the app.
```python
from celery import Celery

app = Celery('tasks', broker='pyamqp://guest@localhost//')

@app.task
def add(x, y):
    return x + y

```

## Running the worker server
```bash
celery -A filename worker --loglevel=info
```
```
[config]
.> app:         tasks:0x10556ad30
.> transport:   redis://localhost:6379//
.> results:     redis://localhost/
.> concurrency: 4 (prefork)
.> task events: OFF (enable -E to monitor tasks in this worker)

[queues]
.> celery           exchange=celery(direct) key=celery
```
- `Broker` is the broker URL.

- `Concurrency` is the number of prefork worker process used to process the tasks concurrently, default to the number of CPU’s on that machine (including cores), experimentation has shown that adding more than twice the number of CPU’s is rarely effective.

- `Events` is an option causes Celery to send monitoring messages (events) for actions occurring in the worker.

- `Queues` is the list of queues that the worker will consume tasks from.

## Calling the task
- import tasks from the module and then run it with the scpecified arguments.
- use `delay`, `apply_async` for asynchronous tasks.
- use primitives to design the workflow.
    - group
    - chain
    - chord
    - map
    - starmap
    - chunks

```python
from tasks import add
add.delay(4, 4)
```

## Keeping results
- need to config the app with `backend` keyword argument.
-- coding: utf-8 --

# anywsgi-py

quick and dirty use any available WSGI server for running from command line

  * https://pypi.org/project/anywsgi/
  * https://github.com/clach04/anywsgi-py

## Getting Started

### Regular install

    pip install anywsgi

### Without a source code checkout

Picking up the latest version

    pip uninstall anywsgi; python -m pip install --upgrade git+https://github.com/clach04/anywsgi-py.git

### From a source code checkout

    # pip uninstall anywsgi
    # python -m pip install -r requirements.txt
    # TODO requirements_optional.txt
    python -m pip install -e .


### Hello World Demo

```python
import os
import sys

import anywsgi
from anywsgi import DEFAULT_LISTEN_ADDRESS, DEFAULT_SERVER_PORT


def application(environ, start_response):
    start_response('200 OK',[('Content-type','text/html')])
    return [b'<html><body>Hello World!</body></html>']  # NOTE Python 3 requires this to be bytes, i.e. an iterator of bytes


print('Python %s on %s' % (sys.version, sys.platform))
listen_address = os.environ.get('LISTEN_ADDRESS', DEFAULT_LISTEN_ADDRESS)
server_port = int(os.environ.get('LISTEN_PORT', DEFAULT_SERVER_PORT))

anywsgi.my_start_server(application, listen_address=listen_address, listen_port=server_port)
```

<details>
<summary>
Hello World Fallback Demo
</summary>
```python
import os
import sys

# Try importing anywsgi, fall back to standard defaults if unavailable
try:
    #raise ImportError  # force failure for debug/testing
    import anywsgi  # from https://github.com/clach04/anywsgi-py
    from anywsgi import DEFAULT_LISTEN_ADDRESS, DEFAULT_SERVER_PORT
except ImportError:
    anywsgi = None
    # Provide standard defaults since anywsgi constants are missing
    DEFAULT_LISTEN_ADDRESS = '127.0.0.1'
    DEFAULT_SERVER_PORT = 8000


def application(environ, start_response):
    start_response('200 OK', [('Content-type', 'text/html')])
    return [b'<html><body>Hello World!</body></html>']  # NOTE Python 3 requires this to be bytes, i.e. an iterator of bytes


print('Python %s on %s' % (sys.version, sys.platform))
listen_address = os.environ.get('LISTEN_ADDRESS', DEFAULT_LISTEN_ADDRESS)
server_port = int(os.environ.get('LISTEN_PORT', DEFAULT_SERVER_PORT))

# Start the server using whichever backend is available
if anywsgi:
    anywsgi.my_start_server(application, listen_address=listen_address, listen_port=server_port)
else:
    from wsgiref.simple_server import make_server

    print("Starting wsgiref HTTP server on http://%s:%d ..." % (listen_address, server_port))
    httpd = make_server(listen_address, server_port, application)
    try:
        httpd.serve_forever()
    except KeyboardInterrupt:
        print("\nServer stopped by user.")
```
</details>

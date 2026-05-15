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

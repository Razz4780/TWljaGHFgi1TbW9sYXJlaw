This is a simple CRUD HTTP server storing key-value pairs.
Server listents on port 8080.
Key must contain only alphanumeric characters. Maximum key length is 100.
Maximal object size is 1MB.

Stored values are preserved after server shutdown and loaded to memory upon server start.

Working Go environment is needed to run this server (developed and tested with Go 1.12)

## Endpoints:

1. ```PUT /api/objects/<id>```
Puts request's body and Content-Type header under key <id>.
Content-Type header is required.
```
$ curl -si 127.0.0.1:8080/api/objects/<key> -XPUT -d 'data' -H 'Content-Type: type'
HTTP/1.1 201 Created
$ curl -si 127.0.0.1:8080/api/objects/<invalid_key> -XPUT -d 'data' -H 'Content-Type: type'
HTTP/1.1 400 Bad Request
$ curl -si 127.0.0.1:8080/api/objects/<key> -XPUT -d '<more than 1MB of data>' -H 'Content-Type: type'
HTTP/1.1 413 Request Entity Too Large
```

2. ```GET /api/objects/<id>```
Retrieves value under key <id>.
```
$ curl -si 127.0.0.1:8080/api/objects/<key>
HTTP/1.1 200 Ok
Content-Type: <content_type>
<object>
$ curl -si 127.0.0.1:8080/api/objects/<absent_key>
HTTP/1.1 404 Not Found
$ curl -si 127.0.0.1:8080/api/objects/<invalid_key>
HTTP/1.1 400 Bad Request
```

3. ```DELETE /api/objects/<id>```
Deletes value under key <id>.
```
$ curl -si 127.0.0.1:8080/api/objects/<key> -XDELETE
HTTP/1.1 204 No Content
$ curl -si 127.0.0.1:8080/api/objects/<absent_key> -XDELETE
HTTP/1.1 404 Not Found
$ curl -si 127.0.0.1:8080/api/objects/<invalid_key> -XDELETE
HTTP/1.1 400 Bad Request
```

4. ```GET /api/objects```
Lists all keys in storage in JSON.
```
 curl -si 127.0.0.1:8080/api/objects
HTTP/1.1 200 Ok
["<key_1>", "<key_2>", "<key_3>"]
```

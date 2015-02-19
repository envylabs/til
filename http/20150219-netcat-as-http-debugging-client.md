# Unix's nc (netcat) as HTTP client for debugging

Quick and easy way to issue an HTTP request:

```bash
$ curl -X PUT -d '{"hello": "world"}' -H "Content-Type: application/json" "http://app.dev/some/path"
```

The above works fine, but adjusting the details is cumbersome, would be
easier if you could use your normal editor to modify these details.

First step along the path is understanding more about what's going on.
Can you issue an HTTP request using a utility that establishes a TCP
connection, like telnet? Yes, you can (but you need to write out the
HTTP request yourself):

```bash
$ telnet app.dev 80
PUT /some/path HTTP/1.1
HOST: app.dev
Accept: */*
Content-Type: application/json
Content-Length: 19

{"hello": "world"}

```

And, then you'll see a response like:

```http
HTTP/1.1 200 OK
Connection: keep-alive
Content-Type: text/html; charset=utf-8
Content-Length: 2

ok
```

Okay, so this works, but it's actually more cumbersome than using cURL.
We can improve on the experience by reading our HTTP request from a file
and piping it to our TCP utility. Unfortunately, we can't continue to
use telnet for that purpose, instead we'll switch to nc (netcat).

First we'll put our HTTP request in a file, we'll name it `request`:

```http
PUT /some/path HTTP/1.1
HOST: app.dev
Accept: */*
Content-Type: application/json
Content-Length: 19

{"hello": "world"}
```

And, then we'll use `nc` to send our request:

```bash
$ cat request | nc app.dev 80
```

Now we can use our favorite editor to modify the request, and any time
we want to issue the request we can re-run that `nc` command.

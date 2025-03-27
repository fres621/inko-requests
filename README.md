# Inko Requests

> [!WARNING]  
> This is a WIP. Some things might not work as described.

This is an Inko package that provides several utility classes and methods to interact with the Internet

## Summary

### `class URL`

The `URL` class lets you parse an uri string to obtain each individual part: `hostname`, `port`, `pathname` and the method `host` which is an alias for `[hostname]:[port]`. This is meant to be similar to the [URL web API](https://developer.mozilla.org/en-US/docs/Web/API/URL), but it's not a 1:1 replica.
Protocols aren't supported as of now.

**Example**

```rb
let url = URL.parse("google.com:443")
stdout.print(url.hostname) # google.com
```

### `fn host_by_name(hostname: String) -> Option[IpAddress]`

> [!WARNING]  
> Use [std.net.dns](https://docs.inko-lang.org/std/main/module/std/net/dns/) instead. host_by_name will be removed in the future.

host_by_name attempts to return an IpAddress from the provided hostname. (Uses [std.net.dns](https://docs.inko-lang.org/std/main/module/std/net/dns/))

**Example**

```rb
let ip = host_by_name("localhost").get
stdout.print(ip.to_string) # 127.0.0.1
```

### `class HTTPRequest`

The `HTTPRequest` class lets you create HTTP requests to pass to a TCP socket.

**Example**

```rb
let url = URL.parse("google.com").get
let ip = host_by_name(url.hostname).get
let socket = TcpClient.new(ip, url.port).get
# [...]
let request = HTTPRequest.new(HTTPMethod.GET, path: url.pathname)
request.set_header("Host", url.hostname)
socket.send(request.to_string)
# We can now get a response
```

# Listen

## Description

The `listen` function allows you to specify the IP/Host, Port and Protocol for endpoints that your `Server` will listen on. If the protocol is `https` then you can also specify a certificate to bind, even having Pode create a self-signed certificate for you.

The `listen` function will check for administrator privileges on Windows, unless the endpoint you're attempting to listen on is a localhost one.

!!! note
    You can specify multiple endpoints to listen on for HTTP/HTTPS endpoints however, you can only supply a single endpoint for SMTP/TCP

## Examples

### Example 1

The following example will listen on every IP over port 8080 for HTTP requests:

```powershell
Server {
    listen *:8080 http
}
```

### Example 2

The following example will listen on localhost over port 25 for SMTP requests (this will not require administrator privileges):

```powershell
Server {
    listen 127.0.0.1:25 smtp
}
```

### Example 3

The following example will listen on a specific IP address over port 8443 for HTTPS requests; it will also inform Pode to create and bind a self-signed certificate to the IP:Port:

```powershell
Server {
    listen 10.10.1.4:8443 https -cert self
}
```

### Example 4

The following example will listen on a specific host name over port 8080 for HTTP requests:

```powershell
Server {
    listen pode.foo.com:8080 http
}
```

### Example 5

The following example will listen on a wildcard endpoint over port 8080 for HTTP requests:

```powershell
Server {
    listen *.foo.com:8080 http
}
```

### Example 6

The following example will listen on multiple endpoints for HTTP (Note, you can specify a combination of HTTP/HTTPS endpoints):

```powershell
Server {
    listen pode.foo.com:8080 http
    listen pode.bar.com:8080 http
}
```

## Parameters

| Name | Type | Required | Description | Default |
| ---- | ---- | -------- | ----------- | ------- |
| IPPort | string | true | The IP/Host:Port combination for an endpoint that the server should listen on | null |
| Type | string | true | The protocol of the endpoint the server should use: HTTP, HTTPS, SMTP, TCP | null |
| Cert | string | false | The certificate to bind to the endpoint. If the certificate is `self` then Pode will create a self-signed certificate. If the certificate is `*.example.com` then it must be installed to `Cert:/LocalMachine/My` | empty |
| Name | string | false | A unique name for this endpoint, which can be used on the [`route`](../Route) and [`gui`](../Gui) functions | empty |
| Force | switch | false | If supplied, will force the `listen` function to not run the administrator check | false |

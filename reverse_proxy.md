# Nginx as Reverse Proxy

## Reverse Proxy

A reverse proxy sits in front of a server and can

- Act as Load Balancer
- Protect the origin servers from attacks by hiding it's IP addresses
- Cache content for faster performance
- Provide SSL encryption and decryption to and from the server

## Directive Needed

Nginx uses `proxy_pass` Directive for configuring reverse proxy.

- Syntax: proxy_pass <destination>
- Request: https://www.example.com

```bash
  location / {
    proxy_pass http://10.1.1.3:9000/app1
  }
```

Nginx uses proxy_pass directory to pass the request to 'example.com' to another server with ip address of `10.1.1.3:9000`.

## References

- https://www.youtube.com/watch?v=PEOzUckp8CI&list=PLHXG_yQQf1HVFWNsZyxIASDCJMjkRUWuR&index=7
- https://www.youtube.com/watch?v=lZVAI3PqgHc&list=PLHXG_yQQf1HVFWNsZyxIASDCJMjkRUWuR&index=9&t=19s

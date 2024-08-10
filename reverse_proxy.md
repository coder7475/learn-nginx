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

**Important**: When nginx starts connection with another server(backend) with proxy pass. It **closes** the previous connection with the client which results in some request information is **lost**. So one should **capture** the original details like actual ip address of client, host details etc and **forward** to the backend server.

## Forward client details to backend server via reverse proxy

One need to Redefining Request Headers and forward it to backend server. To redefine request header use `proxy_set_header`.

### Use proxy_set_header

- **Syntax**: proxy_set_header <HTTP request header field_name> <new_value>;

```bash
  server {
    listen 80;
    proxy_set_header Host $host; # replaces host header that comes from client
    proxy_set_header X-Real-IP $remote_addr; # captures original ip address of requester and sends it to backend
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; # all the traverse route the request takes

    location / {
      proxy_pass http://10.1.1.3:9000/app1
    }
  }
```

## References

- https://www.youtube.com/watch?v=PEOzUckp8CI&list=PLHXG_yQQf1HVFWNsZyxIASDCJMjkRUWuR&index=7
- https://www.youtube.com/watch?v=lZVAI3PqgHc&list=PLHXG_yQQf1HVFWNsZyxIASDCJMjkRUWuR&index=9&t=19s
- https://nginx.org/en/docs/http/websocket.html

## Set Nginx as Reverse Proxy in Ubuntu

- A reverse proxy is the recommended method to expose an application backend server to the internet

- From a client’s perspective, interacting with a reverse proxy is no different from interacting with the application server directly.

#### Benefits of Reverse proxy

- Security benefits in isolating the application server from direct internet access

- The ability to centralize firewall protection

- a minimized attack plane for common threats such as denial of service(DDos) attacks.

## What can Reverse Proxy Do?

A reverse proxy sits in front of a server and can

- Protect the origin servers from attacks by hiding it's IP addresses
- Cache content for faster performance
- Provide SSL encryption and decryption to and from the server
- Act as Load Balancer

## Directive Needed

Nginx uses `proxy_pass` Directive for configuring reverse proxy.

- Syntax:

```bash
proxy_pass <destination>
```

- Example:

```bash
  location / {
    proxy_pass http://10.1.1.3:9000/app1
  }
```

Nginx uses proxy_pass directory to pass the request to another server with ip address of `10.1.1.3:9000`.

**Important**: When nginx starts connection with another server(backend) with proxy pass. It **closes** the previous connection with the client which results in some request information is **lost**. So one should **capture** the original details like actual ip address of client, host details etc and **forward** to the backend server.

## Forward client details to backend server via reverse proxy

One need to Redefining Request Headers and forward it to backend server. To redefine request header use `proxy_set_header`.

### Use proxy_set_header

- **Syntax**:

```bash
proxy_set_header <HTTP request header field_name> <new_value>;
```

- Example of reverse proxy config:

```bash
  // /etc/nginx/sites-available/your_domain
  server {
    listen 80;
    listen [::]:80;

    server_name your_domain www.your_domain;

    location / {
      proxy_pass http://your_server_address;
      include proxy_params;
    }
  }
```

- This is proxy params:

```bash
   // /etc/nginx/proxy_param

    proxy_set_header Host $http_host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;

    # For JWT token
    proxy_set_header Authorization $http_authorization;
    # Web sockets if needed
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
```

Handling CORS:

```bash
const cors = require('cors');

app.use(cors({
  origin: 'http://your-frontend-domain.com',
  credentials: true, // Allow cookies and JWT tokens to be sent
  allowedHeaders: 'Authorization, Content-Type'
}));
```

configure SSL:

```bash
  server {
    listen 443 ssl;
    server_name your-domain.com;

    ssl_certificate /etc/letsencrypt/live/your-domain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/your-domain.com/privkey.pem;

    location /api/ {
        proxy_pass http://your-express-server:your-express-port;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        proxy_set_header Authorization $http_authorization;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}

server {
    listen 80;
    server_name your-domain.com;
    return 301 https://$host$request_uri;
}

```

## References

- https://www.youtube.com/watch?v=PEOzUckp8CI&list=PLHXG_yQQf1HVFWNsZyxIASDCJMjkRUWuR&index=7

- https://www.youtube.com/watch?v=lZVAI3PqgHc&list=PLHXG_yQQf1HVFWNsZyxIASDCJMjkRUWuR&index=9&t=19s

- https://nginx.org/en/docs/http/websocket.html

- https://www.digitalocean.com/community/tutorials/how-to-configure-nginx-as-a-reverse-proxy-on-ubuntu-22-04

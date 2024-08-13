code # Basic Example Config to get started

```bash
// /etc/nginx/conf.d/file_name.conf

server {
  listen 80 default_server;
  server_name tech.local;

  root /var/www/tech.local;
  index index.html index.htm;

  location / {
    try_files $uri $uri/ $uri.html =400;
  }

  location /foss {
    try_files $uri /foss.html;
  }

  error_page 400 404 /400.html;
  location = /400.html {
    internal;
  }

  error_page 500 502 503 504 /50x.html;
  location = /50x.html {
    internal;
  }
}
```

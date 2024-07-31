```bash
server {
  listen 80;
  server_name localhost;

  location / {
    root /usr/share/nginx/html;
    index index.html index.htm;
  }

  # redirect server error pages to the static page /50.x html
  location = /50x.html {
    root /usr/share/nginx/html;
  }
}
```

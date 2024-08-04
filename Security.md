# Security features in NGINX

- Restrict access wherever possible by allowing and blocking IP address.

- You can protect sensitive pages by username

- Use SSL certificate to secure site by encrypting client-server traffic

## Restrict Access Via IP address

Let's say you wanna restrict all api address fromm accessing a page called `secure`. \

To do that in your nginx configuration add a location block inside your server context.

```bash
server {
  ...
  location /secure {
    try_files $uri /secure.html;
    deny all;
  }
  ...
}
```

The `deny all` makes all ip address restricted.

Now test your nginx config and reload. Run:

```bash
  sudo nginx -t
  sudo nginx -s reload
```

Try to access `/secure` path you will get a 403 Forbidden HTTP Status Page.

If you wanna allow any ip address use the below syntax in config file:

```bash
server {
  ...
  location /secure {
    try_files $uri /secure.html;
    allow 127.0.0.1;
    deny all;
  }
  ...
}
```

change `127.0.0.1` to the ip address you want to allow.

If you want to know your own ip address. Go to google and search `what is my ip address`. You will get your ip address from search result.

**Note**: Your IP address is generally a public ip address provided by your ISP provider. It usually changes every one or two days.

### References

- https://www.youtube.com/watch?v=1BlUS5TzSzk&list=PLHXG_yQQf1HVFWNsZyxIASDCJMjkRUWuR&index=7&t=1s

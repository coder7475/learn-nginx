# Security features in NGINX

- Restrict access wherever possible by allowing and blocking IP address.

- You can protect sensitive pages by username and password

- Use SSL certificate to secure site by encrypting client-server traffic

## Restrict Access Via IP address

Let's say you wanna restrict all api address fromm accessing a page called `secure`.

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

## Protect sensitive pages by username and password

Sometimes we want allow internal members to access some page. In that case, we can setup a username and password to allow only internal members.

Steps to setup username and password for a page:

1. First install a utility to set user name and password:

```bash
  sudo apt-get install -y apache2-utils
```

2. Next switch to root user if you are not already. Run:

```bash
  sudo su
```

Enter your password if terminal asks for one.

3. For first time to set a username password.
   Syntax:

```bash
  htpasswd -c /path/to/save/passwords username
```

Example:

```bash
  htpasswd -c /etc/nginx/password admin
```

The above command create a user named `admin`. The terminal will asks you for new password. Type the new password you want to set.

4. If you to add another user called `user1` type:

```bash
  htpasswd /etc/nginx/password user1
```

**Note**: no need for `-c` flag

5. Now let's say we want to set auth for `secure` page. Go to your nginx config file and use:

```bash
# my file path: /etc/nginx/conf.d/tech.conf
# your might be in: /etc/nginx/sites-available/tech.conf
server {
  ...
  location /secure {
        try_files $uri /secure.html;
        auth_basic "Authentication is required here...";
        auth_basic_user_file /etc/nginx/passwords;
        ....
   }
   ....
}
```

6. Now test and reload your nginx config file. Run:

```bash
  sudo nginx -t
  sudo nginx -s reload
```

7. Go to secure page and you will see, it is asking you for username and password.

### References

- https://www.youtube.com/watch?v=1BlUS5TzSzk&list=PLHXG_yQQf1HVFWNsZyxIASDCJMjkRUWuR&index=7&t=1s

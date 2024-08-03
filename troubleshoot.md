# How To Troubleshoot Common Nginx Errors

Common commands to troubleshoot nginx:

- `sudo cat /var/log/nginx/error.log`: This is used to print a log with a list of errors and details about them.
- `sudo nginx -t`: This is used to check for syntax error in configuration.
- `systemctl status nginx`: This is used to check if the Nginx service is active or inactive.

## Troubleshooting with the Error Log

To get a full overview of the nginx errors happening, run:

```bash
   sudo cat /var/log/nginx/error.log
```

Above command results in error log. Example:

```bash
2022/11/28 23:58:22 [emerg] 168641#168641: invalid host in "[::]443" of the "listen" directive in /etc/nginx/sites-enabled/test.do-community.com:12
2022/11/28 23:59:44 [emerg] 168664#168664: invalid number of arguments in "root" directive in /etc/nginx/sites-enabled/test.do-community.com:4
2022/11/29 00:00:19 [emerg] 168701#168701: "server" directive is not allowed here in /etc/nginx/sites-enabled/test.do-community.com:6
2024/08/03 10:41:37 [notice] 28504#28504: using inherited sockets from "6;7;"
2024/08/03 10:52:29 [notice] 31898#31898: signal process started
2024/08/03 11:02:04 [error] 31899#31899: *5 open() "/var/www/tech.local/foss" failed (2: No such file or directory), client: 127.0.0.1, server: tech.local, request: "GET /foss HTTP/1.1", host: "localhost", referrer: "http://localhost/"
2024/08/03 11:02:45 [error] 31899#31899: *5 open() "/var/www/tech.local/foss" failed (2: No such file or directory), client: 127.0.0.1, server: tech.local, request: "GET /foss HTTP/1.1", host: "localhost", referrer: "http://localhost/"
2024/08/03 11:05:54 [error] 31899#31899: *7 open() "/var/www/tech.local/foss" failed (2: No such file or directory), client: 127.0.0.1, server: tech.local, request: "GET /foss HTTP/1.1", host: "localhost", referrer: "http://localhost/"
2024/08/03 11:28:19 [notice] 43309#43309: signal process started
2024/08/03 11:28:30 [crit] 43310#43310: *31 connect() to unix:/this/is/error failed (2: No such file or directory) while connecting to upstream, client: 127.0.0.1, server: tech.local, request: "GET /500error HTTP/1.1", upstream: "fastcgi://unix:/this/is/error:", host: "localhost"
2024/08/03 11:37:53 [crit] 43310#43310: *37 connect() to unix:/this/is/error failed (2: No such file or directory) while connecting to upstream, client: 127.0.0.1, server: tech.local, request: "GET /500error HTTP/1.1", upstream: "fastcgi://unix:/this/is/error:", host: "localhost"
```

The log provides **key information** about the specific error.
The error message is divided into:

- first part: date and time
- second part [...]: provides types of error message, e.g: notice, crit, error, emerg(emergency) etc.
- last part: error message itself (with what file and specific line can be found)

## Checking for Syntax Errors

Most common errors nginx has to do with syntax in configuration error. To check syntax error, run:

```bash
  sudo nginx -t
```

The output if everything is ok:

```bash
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

If there is syntax errors, example of log:

```bash
[emerg] invalid host in "[::]443" of the "listen" directive in /etc/nginx/sites-enabled/test.do-community.com:12
nginx: configuration file /etc/nginx/nginx.conf test failed
```

After verifying syntax is correct always reload nginx. Command:

```bash
  sudo systemctl reload nginx
  # OR
  sudo nginx -s reload
```

## Troubleshooting with `systemctl status nginx`

Always verify if nginx service is active and working on your system. Use `systemd` init system:

```bash
  systemctl status nginx
```

If your service is running, it will read as `active (running)` in output.

If your service is not running, it will read as `inactive (dead)` in the output. If that happens restart Nginx service:

```bash
  sudo systemctl restart nginx
```

Verify if it has restarted with:

```bash
  systemctl status nginx
```

### References

- https://www.digitalocean.com/community/tutorials/how-to-troubleshoot-common-nginx-errors?ref=dailydev

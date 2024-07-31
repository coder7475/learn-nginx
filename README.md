# Common Configurations

Config file is in nginx_config.

Default Config in /conf.d/default.conf.

## User

```bash
user www-data; # a user name 'www-data' uses this config for the webserver
```

**Linux command to find the user called _www-data_**

```bash
cat /etc/passwd | grep www-data
```

## Worker

```bash
worker_processes auto; # how many worker process runs for nginx in the background
```

Nginx recommends setting the worker process to number of cores available

**Linux Command to find numbers of cores**

```bash
cat /proc/cpuinfo | grep cores
```

## PID

```bash
pid /run/nginx.pid;
```

## Error Logs

```bash
error_log /var/log/nginx/error.log;
```

**Linux command to monitor and display error logs added to to error.log**

```bash
tail -f /var/log/nginx/error.log;
```

## Find Default Nginx Page HTML

**Linux Command**

```bash
cat /usr/share/nginx/html/index.html
```

## Location Directive

Given below configuration:

```bash
 /etc/nginx/sites-available/example.com

 location / {
  root /srv/www/example.com/public_html;

  index index.html index.htm;
 }

  location ~\.pl$ {
  gzip off;
  include /etc/nginx/fastcgi_params;

  fastcgi_pass unix:/var/run/fcgiwrap.socket;
  fastcgi_index index.pl;

  fastcgi_param SCRIPT_FILENAME /srv/www/example.com/public_html$fastcgi_script_name;
}
```

In this NGINX configuration, all requests that end in a .pl extension are handled by the second location block, which specifies a fastcgi handler for these requests. Otherwise, NGINX configuration uses the first location directive.

Let’s analyze how NGINX handles some requests based on this configuration:

1.  http://example.com/ is requested - if it exists NGINX returns /srv/www/example.com/public_html/index.html. If the file doesn’t exist, it serves /srv/www/example.com/public_html/index.htm. A 404 is returned if neither exist.

2.  http://example.com/blog/ is requested - if the file exists, /srv/www/example.com/public_html/blog/index.html is returned. If not, /srv/www/example.com/public_html/blog/index.htm is served instead. If neither exist, a 404 error is returned.

3.  http://example.com/tasks.pl is requested - NGINX uses FastCGI handler to execute the file at /srv/www/example.com/public_html/tasks.pl and return the result.

4.  http://example.com/username/roster.pl is requested - NGINX uses FastCGI handler to execute the file at /srv/www/example.com/public_html/username/roster.pl and return the result.

## Reverse Proxy

A reverse proxy accepts requests from clients and forwards the request to servers for the actual processing. The reverse proxy relays the results from servers to the client.

## Load Balancer

A load balancer distributes client requests among a group of backend servers and then relays the response from the selected server to the appropriate client.

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

# Explanation of nginx common configurations

Config file is in nginx_config. All empty lines are skipped.

## First Line

```bash
user www-data; # a user name 'www-data' uses this config for the webserver
```

**Linux command to find the user called _www-data_**

```bash
cat /etc/passwd | grep www-data
```

## Second Line

```bash
worker_processes auto; # how many worker process runs for nginx in the background
```

Nginx recommends setting the worker process to number of cores available

**Linux Command to find numbers of cores**

```bash
cat /proc/cpuinfo | grep cores
```

## Fourth Line

```bash
pid /run/nginx.pid;
```

## Forty One

```bash
error_log /var/log/nginx/error.log;
```

**Linux command to monitor and display error logs added to to error.log**

```bash
tail -f /var/log/nginx/error.log;
```

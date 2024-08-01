# NGINX

## Use Cases

A single instance of nginx can be configured to :

1. Web Server
2. Reverse Proxy
3. Load Balancer
4. Cache
5. Web Application Firewall
6. Internal DDOS Protection
7. API Gateway
8. K8s IC
9. Sidecar Proxy

## Basic Commands

- Check NGINX version

```bash
   nginx -v
```

- Check Configuration file syntax before reloading

```bash
   nginx -t
```

- Display Current Configuration

```bash
  nginx -T
```

- Reload NGINX

```bash
  nginx -s reload
```

## Default location of NGINX configuration

- Main File

```bash
    /etc/nginx/nginx.conf
```

- Includes

```bash
    /etc/nginx/conf.d/*.conf
```

## Configuration Contexts

**Each NGINX configuration has:**

- One Main Main Context
- One HTTP Context

```bash
# Overview
|Main |
      |___|Events|
      |
      |___|HTTP|
      |        |
      |        |____|Server|
      |        |           |_______|Location|
      |        |
      |        |____|Upstream|
      |
      |
      |___|Stream|
                 |
                 |_____|Server|
                 |
                 |_____|Upstream|
```

### Main Context

**Highest Level Directives**:

- Number of worker processes
- Linux Username
- Process ID (PID)
- Log file Location

#### Events Context

**Contains Connection processing directives**:

- Number of connections per worker process

#### HTTP Context

**Determines how NGINX handles HTTP & HTTPS connections**:

- Address of pool of back end servers

##### Server Context

**Define virtual server that responds to a request for:**

- Domain name
- IP address
- Unix Socket

##### Location Context

**Defines how NGINX responds to an HTTP request based on requested URI:**

- Point to Point Matching
- String Matching

##### Upstream Context

**Defines a group of backend servers:**

- Application Servers
- Web Servers

This is essentially for load balancing use case.

#### Stream Context

**Defines handling of Layer 3 and Layer 4 traffic**:

- UDP
- TCP

## NGINX Config: Directives & Blocks

The location of all NGINX configuration files is in the /etc/nginx/ directory. The primary NGINX configuration file is /etc/nginx/nginx.conf.

To set NGINX configurations, use:

1. **Directives** - a statement that controls NGINX Behavior. NGINX configuration options. They tell NGINX to process actions or know a certain variable, such as where to log errors.

2. **Blocks** (also known as _contexts_) - Groups in which Directives are organized

Note that any character after **#** in a line becomes a comment. And NGINX does not interpret it.

**For More about configuration contexts Follow:** https://www.digitalocean.com/community/tutorials/understanding-the-nginx-configuration-file-structure-and-configuration-contexts

## Common Configurations

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

_NGINX_ reverse proxy acts as an intermediate proxy that takes a client request and passes it to the servers. From load balancing, increased security to higher performance - it is used for a range of use cases.

The benefits of reverse proxy are the following:

- **Security**: With a reverse proxy, clients will not have information about our backend servers, so there is no way any **malicious client** cannot access them directly to **exploit any vulnerabilities**. Many reverse proxy servers provide features that help **protect backend servers** from _DDoS attacks_, _IP address blacklisting_ to _reject traffic_ from particular client IP, or _Rate Limit_, which is limiting the number of connections accepted from each client.

- **Optimized SSL Encryption/Decryption**: Implementing SSL/TLS can significantly impact backend server performance because the **SSL handshake** and **encrypting/decrypting** operation for each request is quite **CPU-intensive**.

- **Scalability and high availability**: Along with **load balancing**, the reverse proxy allows to **scale/ add/remove servers** to backend pool based on traffic volume as clients see only the reverse proxy’s IP address. This makes you achieve high availability for your websites/API.

## Load Balancer

A load balancer distributes client requests among a group of backend servers and then relays the response from the selected server to the appropriate client.

- Load balancers help to eliminate a **single point of failure**, making the website/API more reliable by allowing the website/API to be deployed in multiple backend servers.

- Load balancers enhance the _user experience_ by **reducing the number of error responses** for clients either by detecting when one of the backend servers goes down and **diverting requests** away from that server to the other servers in the backend pool or by allowing an application health check.

- Load balancer makes sense when we have **multiple backend servers** because it often makes sense to deploy a reverse proxy even with just one web server or application server.

## NGINX Configuration of Reverse Proxy

To get started with configuring a reverse proxy, follow these steps:

1. If not already installed, install NGINX:

```bash
  sudo apt update
  sudo apt install nginx
```

2. Deactivate your virtual host. To deactivate your virtual host run

```bash
   unlink /etc/nginx/sites-enabled/default
```

3. Change your directory to _/sites-available_ and create a reverse proxy there:

```bash
   cd /etc/nginx/sites-available
   nano reverse-proxy.conf
```

4. Configure proxy server to redirect all requests on port 80 to a lightweight http server that listens to port 8000. To do that, write the following Nginx configuration:

```bash
  server {
    listen 80;
    listen [::]:80;

    access_log /var/log/nginx/reverse-access.log;
    error_log /var/log/nginx/reverse-error.log;

    location / {
      proxy_pass http://127.0.0.1:8000;
    }
  }
```

5. Use the symbolic link and copy configuration from /etc/nginx/sites-available to /etc/nginx/sites-enabled:

```bash
  ln -s /etc/nginx/sites-available/reverse-proxy.conf /etc/nginx/sites-enabled/reverse-proxy.conf
```

6. Verify if NGINX is working:

```bash
  nginx -t
```

If you see a successful test message, NGINX reverse proxy is properly configured on your system.

## Configuring Load Balance with NGINX

To configure your NGINX and use it as a load balancer:

1. Add your backend servers to your configuration file first. Collect your server IPs that acts as load balancers:

```bash
# File: load_balancer.conf
upstream backend {
  server 72.229.28.185;
  server 72.229.28.186;
  server 72.229.28.187;
  server 72.229.28.188;
  server 72.229.28.189;
  server 72.229.28.190;
}
```

2. After upstream servers are defined, go to the location /etc/nginx/sites-available/ and edit load_balancer.conf.

```bash
# File: /etc/nginx/sites-available/load_balancer.conf

upstream backend {
  server 72.229.28.185;
  server 72.229.28.186;
  server 72.229.28.187;
  server 72.229.28.188;
  server 72.229.28.189;
  server 72.229.28.190;
}

server {
    listen 80;
    server_name SUBDOMAIN.DOMAIN.TLD;
    location / {
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_pass https://backend;
    }
}
```

Every time a request is made to port 80 to SUBDOMAIN.DOMAIN.LTD, request is routed to upstream servers.

3. Execute this new configuration by reloading NGINX using

```bash
  nginx -t
  cd /etc/nginx/site-enabled/
  ln -s ../sites-available/load_balancer.conf
  systemctl reload nginx
```

You can further **configure and optimize** your load balancer by load balancing methods like _Round Robin_, _Least connected_, _IP hash_, and _Weighted_.

**Note**: For more configuration follow: https://www.linode.com/docs/guides/how-to-configure-nginx/

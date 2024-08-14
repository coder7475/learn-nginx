## How to Uninstall nginx in ubuntu

1.Log into your server

```bash
  ssh root@your_server_ip_address
```

2. Stop the webserver

```bash
  sudo systemctl stop nginx
```

3. Uninstall using `purge` command

```bash
  sudo apt purge nginx
```

4. Remove any dependencies installed along nginx

```bash
  sudo apt autoremove
```

5. Verify uninstallation using

```bash
 sudo systemctl status nginx
```

# Nginx by Mayahn Ahuja from Linkedin Post

My doctor told me to reduce stress. I replaced Apache with Nginx. 😉

Nginx (pronounced "engine-x") ->

- Web Server
- Reverse Proxy
- Load Balancer

⭐ NGINX is a reverse proxy load balancer, meaning it manages connections between clients and backend servers.

◾ It is free & open-source.

◾ It's renowned for its efficiency, stability and ability to handle massive loads concurrently.

𝐔𝐧𝐝𝐞𝐫 𝐭𝐡𝐞 𝐡𝐨𝐨𝐝 -

◾ At its heart, Nginx is an event-driven web server.

◾ It doesn't dedicate a thread to each incoming connection (like traditional models).

◾ Instead, it relies on a single (or a few) worker processes to manage multiple connections concurrently.

◾ Nginx utilizes a non-blocking I/O model -

- Nginx uses => efficient system calls (like epoll on Linux ) => to watch multiple file descriptors (representing connections, sockets, etc.).
- When a connection is ready, Nginx performs the read/write operation asynchronously.
- It doesn't wait for the operation to complete.
- Instead, it delegates the task to the 'operating system' and immediately moves on to handle other events.
- When the asynchronous operation finishes, the operating system triggers a callback function in Nginx.
- This callback then processes the data and potentially schedules more asynchronous operations.

This enables NGINX to handle thousands of simultaneous connections with minimal resources.

𝐌𝐚𝐬𝐭𝐞𝐫 & 𝐖𝐨𝐫𝐤𝐞𝐫 𝐏𝐫𝐨𝐜𝐞𝐬𝐬𝐞𝐬 -

◾ The master process sets up the infrastructure (like listening sockets) and delegates the actual connection handling and request processing to the worker processes.

◾ Each worker process then runs its own independent event loop, utilizing the non-blocking I/O model.

So,

Worker processes does the actual heavy lifting of handling client requests and interacting with backend servers.

◾ This separation allows Nginx to handle numerous concurrent connections efficiently.

Remember,

Nginx strikes a balance between both approaches -

# Worker Processes (Multi-Process)

# Event-Driven Within Each Process (Not Multi-Threaded)

(Inside each worker process, the event-driven, non-blocking model comes into play.)

𝐊𝐞𝐲 𝐅𝐮𝐧𝐜𝐭𝐢𝐨𝐧𝐚𝐥𝐢𝐭𝐢𝐞𝐬 -

# Web Serving

Nginx serves static content (HTML, CSS, images) efficiently.

# Reverse Proxy

Nginx forwards requests to backend servers while potentially adding security or caching functionalities.

Can handle SSL/TLS termination (encryption/decryption).

# Load Balancer (Layer 4 & 7)

Nginx can distribute traffic across multiple backend servers using various algorithms (round-robin, least connections, etc.)

Improves scalability and high availability.

---

Remember,

◾ Nginx performance can be impacted by long-running requests that block the event loop.
◾ For computationally intensive tasks, Nginx may benefit from using threads or offloading work to external processes. 😊

### Reference

- https://www.linkedin.com/feed/update/urn:li:activity:7225758044226281472/
  s

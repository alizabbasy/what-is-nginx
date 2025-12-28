# Nginx Types and Use Cases

A beginner-friendly guide to understanding Nginx, its types, and use cases, designed for DevOps and infrastructure engineers in training.

---

## 1️⃣ What is Nginx?

**Nginx** is a high-performance web server, reverse proxy, and load balancer. It is widely used because it is:

* Fast and efficient
* Event-driven (handles many connections simultaneously)
* Lightweight and easy to scale

💡 Think of Nginx as a **traffic controller** for web requests: it decides where requests go and how they are handled.

---

## 2️⃣ Types / Uses of Nginx

Nginx can be used in several roles:

1. **Web Server** (serving static content)
2. **Reverse Proxy** (forward requests to backend servers)
3. **Forward Proxy** (clients go through Nginx to access external sites)
4. **Load Balancer** (distribute traffic across multiple servers)
5. **API Gateway** (manage APIs, routing, and security)
6. **Caching Server** (store responses to reduce backend load)

---

## 3️⃣ Web Server

**Role:** Serve static files (HTML, CSS, JS, images) directly to clients.

**Example Config:**

```nginx
server {
    listen 80;
    server_name example.com;

    root /var/www/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

**Explanation:**

* Nginx reads files from disk and sends them to clients.
* No backend processing needed.

---

## 4️⃣ Reverse Proxy

**Role:** Forward client requests to backend servers (e.g., Node.js, PHP-FPM, Python).

**Example Config:**

```nginx
server {
    listen 80;
    server_name example.com;

    location /api/ {
        proxy_pass http://127.0.0.1:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

**Explanation:**

* Client requests to `/api/` are forwarded to backend server.
* Backend handles processing and Nginx passes the response back.

---

## 5️⃣ Forward Proxy

**Role:** Nginx acts on behalf of clients to access external sites (not very common for public web servers).

**Example Config:**

```nginx
server {
    listen 3128;

    location / {
        proxy_pass http://$http_host$request_uri;
    }
}
```

**Explanation:**

* Client sends request to Nginx.
* Nginx fetches content from external site and returns it.

---

## 6️⃣ Load Balancer

**Role:** Distribute incoming traffic across multiple backend servers.

**Example Config:**

```nginx
upstream backend {
    server 192.168.0.101;
    server 192.168.0.102;
}

server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://backend;
    }
}
```

**Explanation:**

* Requests are sent to `backend` upstream, which balances load between servers.
* Helps prevent overload on a single server.

---

## 7️⃣ API Gateway

**Role:** Manage API requests, routing, authentication, and security.

**Example Config:**

```nginx
server {
    listen 80;
    server_name api.example.com;

    location /v1/ {
        proxy_pass http://backend_v1;
        # Add headers or rate limits if needed
    }
}
```

**Explanation:**

* Nginx routes API requests and can add authentication, rate limiting, or caching.

---

## 8️⃣ Caching Server

**Role:** Cache responses to reduce load on backend servers.

**Example Config:**

```nginx
proxy_cache_path /tmp/nginx_cache levels=1:2 keys_zone=my_cache:10m max_size=1g;

server {
    listen 80;
    server_name example.com;

    location / {
        proxy_cache my_cache;
        proxy_pass http://backend;
    }
}
```

**Explanation:**

* Responses from backend are cached on Nginx.
* Subsequent requests are served from cache, reducing backend load.

---

This document introduces the main **roles of Nginx** with simple explanations and example configurations. Advanced configuration and optimization will be covered in future documents.


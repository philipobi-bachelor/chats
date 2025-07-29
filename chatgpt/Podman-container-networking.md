---
Source: https://chatgpt.com/c/686c351a-6d10-8008-a8b9-12acd0ffeda7
Model: o4-mini
Exported with: https://github.com/pionxzh/chatgpt-exporter
---

# Podman container networking

#### You:
Podman how to access a port on another container when the containers are on the same bridge network

#### ChatGPT:
\[Unsupported Content]


#### ChatGPT:
\[Unsupported Content]


#### ChatGPT:
When you put two (or more) containers on the same user‑defined bridge network in Podman, they can talk to each other over that network without ever “publishing” ports to the host. Here’s how to set it up and verify you can reach a port on one container from another:

---

## 1. Create a user‑defined bridge network

```bash
podman network create app-net
```

This gives you a network called `app-net` that supports container‑to‑container DNS and IP connectivity.

---

## 2. Run your containers on that network

### Example: a simple web service

```bash
# Backend service (e.g. listening on port 5000)
podman run -d \
  --name backend \
  --network app-net \
  quay.io/myorg/my-backend:latest

# Frontend service
podman run -d \
  --name frontend \
  --network app-net \
  quay.io/myorg/my-frontend:latest
```

> **Note:** We did **not** use `-p` or `--publish` here—publishing is only needed when you want to expose ports on the **host**.

---

## 3. Verify internal connectivity

1. **Find the backend’s IP (optional):**
   ```bash
   podman inspect -f '{{.NetworkSettings.Networks.app-net.IPAddress}}' backend
   ```
   Suppose it returns `10.88.0.5`.

2. **From the frontend container, `curl` the backend port:**

   ```bash
   podman exec -it frontend curl http://backend:5000/health
   # or using the IP directly
   podman exec -it frontend curl http://10.88.0.5:5000/health
   ```

   If your service is listening on all interfaces (`0.0.0.0:5000` inside the container), you should get a healthy response.

---

## 4. Common pitfalls

- **Service binding**\
  Make sure your server inside the container binds to `0.0.0.0`, *not* `127.0.0.1`. If it only listens on localhost, other containers won’t reach it.

- **Rootless vs. rootful**\
  In rootless mode, Podman still supports user‑defined networks, but you’ll need a recent Podman (v4+) and CNI support. The same steps apply.

- **DNS vs. IP**\
  You almost always want to use container names (`backend`, `db`, etc.) because Docker‑style DNS will resolve them for you. Hard‑coding IPs can break if you recreate containers.

---

### Quick recipe

1. `podman network create mynet`
2. `podman run -d --name A --network mynet imageA`
3. `podman run -d --name B --network mynet imageB`
4. From B: `curl http://A:PORT`

That’s it—containers on the same bridge network can see each other on their exposed ports without any host‑port magic.

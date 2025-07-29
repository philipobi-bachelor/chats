---
Source: https://chatgpt.com/c/683f2992-72b4-8008-856f-4e36c94b8c15
Model: gpt-4o
Exported with: https://github.com/pionxzh/chatgpt-exporter
---

# Rootless Podman Quadlet Setup

#### You:
using quadlets as rootless user

#### ChatGPT:
Running **Quadlets** as a **rootless user** is possible and allows non-root users to manage containers via systemd unit files (typically using `podman` instead of Docker). Here’s a step-by-step guide for setting up **Podman quadlets** as a **rootless user**.

---

## ✅ Requirements

1. **Podman** 4.3+ installed.
2. **Systemd** user services enabled.
3. A directory for user systemd units (e.g., `~/.config/systemd/user/`).
4. Your distro should support systemd user units and rootless Podman (most modern distros do).

---

## 🏗️ Steps to Use Quadlets as a Rootless User

### 1. ✅ Create Quadlet Unit File

Quadlets are `.container`, `.volume`, `.network`, etc., files that systemd parses into full unit files.

For example, create a file:

```bash
mkdir -p ~/.config/containers/systemd/
nano ~/.config/containers/systemd/myapp.container
```

### 2. ✍️ Example Quadlet `.container` File

```ini
[Unit]
Description=My Rootless App Container

[Container]
Image=nginx:alpine
PublishPort=8080:80
Exec=nginx -g "daemon off;"
```

You can also specify:

- `Volume=`
- `Network=`
- `User=` (typically omitted in rootless)
- `Environment=`

---

### 3. 🛠️ Generate and Enable Systemd Unit

```bash
systemctl --user daemon-reload
systemctl --user enable --now myapp.container
```

---

### 4. ✅ Confirm It's Running

```bash
systemctl --user status myapp.container
```

Or check the container directly:

```bash
podman ps --all
```

---

## 🔄 Updating the Quadlet

Edit the `.container` file, then:

```bash
systemctl --user daemon-reload
systemctl --user restart myapp.container
```

---

## 📝 Notes

- All services run under the **user’s UID**, not as root.
- You must use **Podman**, as Docker does not support rootless systemd integration.
- Use `loginctl enable-linger <username>` if you want the service to run when you're not logged in.

---

Would you like a complete working example (like with a mounted volume or environment variables)?

# n8n Docker Installation and Setup

## System Overview

- Host OS: [Your host OS here]
- Docker version: [docker --version]
- Docker Compose version: [docker-compose --version]
- Port: <port> (default)

## 1. Install Docker and Docker Compose

```
$ sudo dnf install -y docker docker-compose
$ sudo systemctl enable --now docker
```
---

## 2. Create Docker Compose File

- Create a docker-compose.yml in a dedicated directory
```
version: "3"

 services:
  n8n:
    image: n8nio/n8n
    ports:
      - "<port>:<port>"
    volumes:
      - ~/.n8n:/home/node/.n8n
    restart: unless-stopped
```

---

## 3. Start n8n Container
```
$ docker-compose up -d
```

---

## 4. Verify n8n Container Running
```
$ docker ps
```

> Access n8n web UI at <URL://hostname:port>

---

## 5. Managing the n8n Container

- Restart container:
```
$ docker-compose restart
```

- Stop container:
```
$ docker-compose down
```

- View logs:
```
$ docker-compose logs -f
```

---

## 6. ## SELinux Configuration for n8n (Podman Deployment)

- **Initial State**: SELinux set to `Permissive` for safe testing.
- **Symptoms**: Podman container failed to bind to port `<port>`, and `ausearch` showed no relevant AVC denials initially.
- **Actions Taken**:
  - Set custom SELinux context on bind mount directory:
    ```
    $ sudo chcon -Rt container_file_t /home/user/n8n-docker
    ```
  - Created and applied a custom SELinux policy module (e.g., `n8n_port.te`) to explicitly allow access.
  - Restarted the n8n container and confirmed successful binding and access via `curl`.

- **Verification**:
  - Ran `ausearch -m avc -ts recent` post-restart.
  - **Result**: No AVC denials observed. SELinux is not blocking container operations.
  - Exported log to: `~/selinux-n8n-audit.log` (empty)
  - n8n now runs cleanly under SELinux constraints.

---

## 7. Next Steps

- Configure environment variables via .env
- Set up revers proxy with SSL
- Add workflow backups

---

## Summary

- Chose Docker to quickly containerize and deploy the n8n automation platform.
- Ensured secure use of `.env` files, custom volumes, and port mappings.
- Verified persistent data and UI accessibility from the host system.
- This setup showcases containerization fundamentals and environment management.
- Useful for prototyping or running isolated workflows without system-level overhead.


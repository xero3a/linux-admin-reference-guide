# n8n Deployment via Podman (automation/n8n-podman)

This guide outlines the steps for deploying **n8n**, an automation and workflow tool, using **Podman** as the container runtime on a RHEL 9 environment. This approach leverages Podman’s rootless and SELinux-friendly design.

---

### 1. System Overview

- **OS:** RHEL 9 (x86_64)
- **Runtime:** Podman + podman-compose
- **Deployment Method:** docker-compose.yml
- **Directory:** `automation/n8n-podman/`
- **Access Method:** Web UI over HTTP
- **Port:** [Redacted]

---

### 2. Podman Installation

- Install Podman and podman-compose
```
$ sudo dnf install -y podman podman-compose
$ podman --version
$ podman-compose version
```

---

### 3. Project Directory Structure

```
$ mkdir -p ~/projects/n8n-podman
$ cp automation/n8n-podman/docker-compose.yml ~/projects/n8n-podman/
$ cd ~/projects/n8n-podman
```

- Create a .env file for sensitive variables
```
$ touch .env
```
- Populate .env:
```
  	 N8N_BASIC_AUTH_USER=yourusername
	 N8N_BASIC_AUTH_PASSWORD=yourpassword
```

**Note**: Keep `.env` in your .gitignore file.

---

### 4. Starting the Container

- Use Podman Compose to build and run n8n:
```
$ podman-compose up -d
$ podman ps
```

> Visit: http://<host>:<port> to access the n8n UI.

---

### 5. SELinux Support

- If SELinux blocks n8n from binding to the port:

#### Add custom port to SELinux context
```
$ sudo semanage port -a -t http_port_t -p tcp <port>
```

#### Or apply a custom policy (see: n8n.te)
```
$ checkmodule -M -m -o n8n.mod n8n.te
$ semodule_package -o n8n.pp -m n8n.mod
$ sudo semodule -i n8n.pp
```

---

### 6. Common Tasks

- View logs
```
$ podman-compose logs -f
```

- Restart container
```
$ podman-compose restart
```

- Stop and clean up
```
$ podman-compose down
```

---

### 7. Notes & Differences from Docker

---
```
Feature		             | Podman		                   | Docker
_________________________|_________________________________|___________________________
 Rootless containers     | Native support	               | Optional w/ setup         
 SELinux integration     | Full support	     	           | Requires workarounds      
 Systemd integration     | Podman generate systemd         | Needs third-party tooling 
 Daemonless 	         | No central daemon	           | Always-on docker daemon   
---------------------------------------------------------------------------------------
```
---

### 8. Next Steps

- Configure n8n systemd service using podman generate systemd
- Add reverse proxy and SSL (e.g., Nginx + Certbot)
- Create workflow examples for scheduled backups, API polling, etc.

---

### Summary

- Used Podman to deploy n8n in a rootless, SELinux-compatible container environment.
- Created a custom SELinux policy module (`n8n.te`) to allow port binding.
- Focused on security, user isolation, and systemd integration for long-term automation.
- Demonstrates ability to work with enterprise-grade Linux container tooling.
- Valuable for production-grade deployment on hardened or minimal systems.


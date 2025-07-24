# n8n Native Installation on Oracle VirtualBox VM (OVBM)

This guide documents the step-by-step process to install and configure **n8n**, an automation and workflow orchestration tool, on a Linux-based Virtual Machine created with Oracle VirtualBox.

---

## System Overview

- **Host OS:** [Your host OS here]
- **Guest OS:** RHEL 9 (or specify)
- **VM Manager:** Oracle VirtualBox
- **IP Address:** [Insert IP if static or bridged]
- **Access Method:** Localhost or SSH
- **Port:** 5678 (default)

---

## 1. Install Node.js and npm

```bash
$ sudo dnf install -y nodejs npm
$ node -v
$ npm -v

---

## 2. Install n8n Globally

$ sudo npm install n8n -g

**Note:** Note: You may see "213 packages looking for funding" — this is expected for open source packages managed via npm.

---

## 3. Start n8n (initial startup)

$ n8n

	a. Visit: http://localhost:5678
	b. Register your owner account and configure the UI
	c. Optional: enter license key when available

---

## 4. Create a Systemd Service for n8n

$ sudo nano /etc/systemd/system/n8n.service

~ [Unit]
~ Description=n8n workflow automation
~ After=network.target

~ [Service]
~ Type=simple
~ User=sleepy
~ ExecStart=/usr/local/bin/n8n
~ Restart=on-failure
~ Environment=PATH=/usr/bin:/usr/local/bin
~ Environment=N8N_PORT=5678
~ Environment=EDITOR=nano
~ WorkingDirectory=/home/sleepy/

~ [Install]
~ WantedBy=multi-user.target

- Enable and start the service:
$ sudo systemctl daemon-reexec
$ sudo systemctl daemon-reload
$ sudo systemctl enable --now n8n

---

## 5. Permissions Warning

- If you see:
$ Permissions 0644 for /home/sleepy/.n8n/config are too wide.

- Fix with:
$ chmod 600 ~/.n8n/config

---

## 6. Troubleshooting Common Errors

- Port 5678 already in use:
$ sudo lsof -i :5678
$ sudo systemctl stop n8n
$ sudo systemctl start n8n

- n8n already running in the background:
$ ps aux | grep n8n
$ kill -9 [PID]

---

## 7. Verify n8n is Running

sudo systemctl status n8n
**Note:** Visit http://localhost:5678 to confirm the web UI is accessible

---

## 8. What Comes Next...

- Create a backup and restore automation
- Set up SSL with Nginx reverse proxy
- Add workflow examples

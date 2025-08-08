# n8n Native Installation on Oracle VirtualBox VM (OVBM)

This guide documents the step-by-step process to install and configure **n8n**, an automation and workflow orchestration tool, on a Linux-based Virtual Machine created with Oracle VirtualBox.

---

## System Overview

- **Host OS:** [Your host OS here]
- **Guest OS:** RHEL 9 (or specify)
- **VM Manager:** Oracle VirtualBox
- **IP Address:** [Insert IP if static or bridged]
- **Access Method:** <hostname> or <ssh>
- **Port:** <port> (default)

---

## 1. Install Node.js and npm

```bash
$ sudo dnf install nodejs npm
$ node -v
$ npm -v
```

---

## 2. Install n8n Globally
```
$ sudo npm install n8n -g
```

**Note:** Note: You may see "# of packages looking for funding" — this is expected for open source packages managed via npm.

---

## 3. Start n8n (initial startup)
```
$ n8n

a. Visit: <URL://Hostname:Port>
b. Register your owner account and configure the UI
c. Optional: enter license key when available
```
---

## 4. Create a Systemd Service for n8n
```
$ sudo nano /etc/systemd/system/n8n.service

[Unit]
 Description=n8n workflow automation
 After=network.target

 [Service]
 Type=simple
 User=<User_name>
 ExecStart=/usr/local/bin/n8n
 Restart=on-failure
 Environment=PATH=/usr/bin:/usr/local/bin
 Environment=N8N_PORT=<port>
 Environment=EDITOR=nano
 WorkingDirectory=/home/<user>/

[Install]
 WantedBy=multi-user.target
```

- Enable and start the service:
```
$ sudo systemctl daemon-reexec
$ sudo systemctl daemon-reload
$ sudo systemctl enable --now n8n
```

# Open Firewall Port (if firewalld is enabled)

- If `firewalld` is running, you'll need to open the n8n port (default: <port>):
```
$ sudo firewall-cmd --add-port=<port>/<protocol> --permanent
$ sudo firewall-cmd --reload
```

- Verify the port is open
```
$ sudo firewalld-cmd --list-ports
```

---

## 5. Permissions Warning

- If you see:
```
$ Permissions 0644 for /home/user/.n8n/config may be too accessible.
```

- Fix with:
```
$ chmod 600 ~/.n8n/config
```

---

## 6. Troubleshooting Common Errors

- Port ##### already in use:
```
$ sudo lsof -i :<port>
$ sudo systemctl stop n8n
$ sudo systemctl start n8n
```

- n8n already running in the background:
```
$ ps aux | grep n8n
$ kill -9 [PID]
```

- Verify firewalld is running
```
$ sudo systemctl status firewalld
```
	...or...

- Verify firewalld is installed
```
$ sudo dnf info firewalld
```
---

## 7. Verify n8n is Running

```
$ sudo systemctl status n8n
```
> **Note:** Visit <URL://Hostname:Port> to confirm the web UI is accessible

---

## 8. What Comes Next...

- Create a backup and restore automation
- Set up SSL with Nginx reverse proxy
- Add workflow and trigger examples

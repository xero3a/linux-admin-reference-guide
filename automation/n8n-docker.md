# n8n Docker Installation and Setup

## System Overview

- Host OS: [Your host OS here]
- Docker version: [docker --version]
- Docker Compose version: [docker-compose --version]
- Port: 5678 (default)

## 1. Install Docker and Docker Compose

```bash
$ sudo dnf install -y docker docker-compose
$ sudo systemctl enable --now docker

---

## 2. Create Docker Compose File

- Create a docker-compose.yml in a dedicated directory
~ version: "3"
~
~ services:
~  n8n:
~    image: n8nio/n8n
~    ports:
~      - "5678:5678"
~    volumes:
~      - ~/.n8n:/home/node/.n8n
~    restart: unless-stopped

---

## 3. Start n8n Container

$ docker-compose up -d

---

## 4. Verify n8n Container Running

$ docker ps

- Access n8n web UI at http://localhost:5678

---

## 5. Managing the n8n Container

- Restart container:
$ docker-compose restart

- Stop container:
$ docker-compose down
 - View logs:
$ docker-compose logs -f

---

## 6. Next Steps

- Configure environment variables via .env
- Set up revers proxy with SSL
- Add workflow backups

---
title: Docker Commands (Cheatsheets)
tags:
  - Command-Line
  - Containers
  - Services
created: 2025-07-11T14:49:00
---

## 🐳 **Docker CLI Cheatsheet**

### 🚧 1. **Build a Docker Image**

bash

CopyEdit

`docker build -t my-image-name .`

- `-t` → Tag the image (`name:tag`)
    
- `.` → Current directory (Dockerfile should be here)
    

---

### 🚀 2. **Run a Docker Container**

bash

CopyEdit

`docker run -d --name my-container my-image-name`

- `-d` → Detached mode (runs in background)
    
- `--name` → Give your container a name
    

---

### 🌐 3. **Run with Port Mapping**

bash

CopyEdit

`docker run -d -p 8080:80 my-image-name`

- `-p host:container` → Map port 8080 on host to 80 inside container
    

✅ Combine with `--name`:

bash

CopyEdit

`docker run -d --name my-container -p 3000:3000 my-image-name`

---

### 🧪 4. **Run with Shell Access (e.g. `sh` or `bash`)**

bash

CopyEdit

`docker run -it my-image-name sh`

- `-it` → Interactive + TTY
    
- `sh` or `bash` at the end starts a shell session
    

🧠 Useful for Alpine-based containers: use `sh`

---

### 📋 5. **List Docker Containers and Images**

bash

CopyEdit

`docker ps           # Running containers docker ps -a        # All containers (including stopped) docker images       # Local images`

---

### 🧼 6. **Stop / Remove Containers and Images**

bash

CopyEdit

`docker stop my-container docker rm my-container docker rmi my-image-name`

---

## 🛠️ **Docker Compose Cheatsheet**

### 📄 1. **`docker-compose.yml` Minimal Example**

yaml

CopyEdit

`version: '3.8'  services:   app:     build: .     ports:       - "3000:3000"     command: sh  # Optional: override default CMD`

---

### 🔨 2. **Build Docker Compose Services**

bash

CopyEdit

`docker-compose build`

---

### 🚀 3. **Run (Up) All Services**

bash

CopyEdit

`docker-compose up -d`

- `-d` → Detached mode
    

---

### 🛑 4. **Stop Services**

bash

CopyEdit

`docker-compose down`

- Stops and removes containers, networks, etc.
    

---

### 🔍 5. **View Logs**

bash

CopyEdit

`docker-compose logs docker-compose logs -f  # Follow logs`

---

### 📋 6. **List Running Containers (Compose Context)**

bash

CopyEdit

`docker-compose ps`

---

## 🧠 BONUS: Useful Docker Tips

|Task|Command|
|---|---|
|View logs|`docker logs my-container`|
|Get shell in running container|`docker exec -it my-container sh`|
|Copy files into container|`docker cp file.txt my-container:/app/`|
|View container IP|`docker inspect my-container|
|Clean unused stuff|`docker system prune -a`|

---

## ✅ Summary

|Goal|Command|
|---|---|
|**Build image**|`docker build -t my-app .`|
|**Run with port**|`docker run -d -p 8080:80 my-app`|
|**Run with shell**|`docker run -it my-app sh`|
|**Compose up/down**|`docker-compose up -d` / `docker-compose down`|
|**Compose logs**|`docker-compose logs -f`|
# PSUSphere-Docker_Config
# Docker Setup Instructions for psusphere

This guide explains how to build and run the **psusphere** application using Docker.

## Prerequisites
- [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed and running.

## Quick Start (Recommended)

The easiest way to run the application is using Docker Compose. This handles building the image and setting up the correct ports automatically.

1. Open a terminal in the `projectsite` directory:
   ```powershell
   cd projectsite
   ```

2. Build and start the container:
   ```powershell
   docker-compose up --build
   ```

3. Access the application in your browser:
   - **URL**: [http://localhost:8000](http://localhost:8000)

4. To stop the server, press `Ctrl+C` in the terminal.

---

## Manual Build & Run (Advanced)

If you prefer to run Docker commands manually, follow these steps carefully to avoid connection issues.

### 1. Build the Image
Make sure you are in the `projectsite` directory where the `Dockerfile` is located.

```powershell
docker build -t my-django-app:latest .
```
> **Note**: Don't forget the `.` at the end! It tells Docker to use the current directory.

### 2. Run the Container
You **must** map the ports using the `-p` flag, otherwise you won't be able to access the site from your browser.

```powershell
docker run -p 8000:8000 my-django-app:latest
```

- `-p 8000:8000`: Maps port 8000 on your computer to port 8000 inside the container.

---

## Troubleshooting

### "Refused to connect" or "Site can't be reached"
- **Cause**: The container is running, but the port isn't mapped, or the app is listening on `127.0.0.1` instead of `0.0.0.0`.
- **Fix**:
  1. Ensure your `Dockerfile` uses `0.0.0.0:8000` in the CMD command (we fixed this previously).
  2. Ensure you are using `docker-compose up` OR `docker run -p 8000:8000 ...`.
  3. Check if the container is running with `docker ps`.

### "Port is already allocated"
- **Cause**: Another service (or a previous Docker container) is already using port 8000.
- **Fix**:
  1. Stop all running containers:
     ```powershell
     docker stop $(docker ps -q)
     ```
  2. Or, change the port mapping in the command:
     ```powershell
     docker run -p 8080:8000 my-django-app:latest
     ```
     (Then access at `http://localhost:8080`)

### Changes not showing up?
- **Cause**: Docker caches old layers.
- **Fix**: Force a rebuild:
  ```powershell
  docker-compose up --build
  ```

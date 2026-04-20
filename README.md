# Docker A to Z: Theory + Practical Guide

This repository demonstrates a simple Flask app containerized with Docker.  
This README is designed as a complete beginner-to-advanced theory guide and hands-on reference.

## 1. What is Docker?

Docker is a platform to build, package, ship, and run applications in isolated units called containers.

- Container: A lightweight, runnable package containing app code + dependencies + runtime.
- Image: Read-only template used to create containers.
- Dockerfile: Script with instructions to build an image.
- Registry: Storage for images (for example, Docker Hub).

## 2. Why Docker?

Docker solves the classic "it works on my machine" problem.

- Consistent environments across local, test, and production.
- Faster onboarding for teams.
- Better resource utilization than virtual machines.
- Easier CI/CD and deployment workflows.

## 3. Container vs Virtual Machine

- VM virtualizes hardware and runs a full guest OS.
- Container virtualizes at OS level and shares host kernel.
- Containers start faster and consume less memory.
- VMs provide stronger isolation but are heavier.

## 4. Core Docker Architecture

- Docker Client: CLI command interface (`docker ...`).
- Docker Daemon: Background service that builds/runs containers.
- Docker Engine: Client + daemon + REST API.
- Images, Containers, Volumes, Networks: Main building blocks.

## 5. Image Lifecycle

1. Write a Dockerfile.
2. Build image with `docker build`.
3. Run container with `docker run`.
4. Tag image with `docker tag`.
5. Push image with `docker push`.
6. Pull and run anywhere with Docker installed.

## 6. Dockerfile A to Z Concepts

Common instructions:

- `FROM`: Base image.
- `WORKDIR`: Set working directory.
- `COPY`: Copy files into image.
- `RUN`: Execute commands during build.
- `ENV`: Set environment variables.
- `EXPOSE`: Document intended port.
- `CMD`: Default runtime command.
- `ENTRYPOINT`: Fixed executable command.

Best practices:

- Use small base images (like slim/alpine when suitable).
- Use `.dockerignore` to avoid copying unnecessary files.
- Keep layers cache-friendly (copy requirements first, then install).
- Pin dependency versions.
- Run app as non-root user for better security.

## 7. Your Current Dockerfile (Theory Mapping)

Your file [dockerfile](dockerfile) does the following:

1. Uses `python:3.8-slim` as base image.
2. Sets working directory to `/app`.
3. Copies project files into container.
4. Installs Python dependencies from [requirements.txt](requirements.txt).
5. Exposes port `5000`.
6. Sets `FLASK_APP=app.py`.
7. Starts Flask on `0.0.0.0`.

Note: Conventional filename is `Dockerfile` (capital D). Docker can still build with current name if passed explicitly.

## 8. Essential Docker Commands

### Information and setup

- `docker --version`
- `docker info`
- `docker system df`

### Build images

- `docker build -t flask-docker-demo:1.0 -f dockerfile .`

### Run containers

- `docker run -d --name flask-demo -p 5000:5000 flask-docker-demo:1.0`
- `docker ps`
- `docker logs -f flask-demo`
- `docker stop flask-demo`
- `docker rm flask-demo`

### Image management

- `docker images`
- `docker rmi flask-docker-demo:1.0`
- `docker tag flask-docker-demo:1.0 your-dockerhub-username/flask-docker-demo:1.0`

### Push/Pull from registry

- `docker login`
- `docker push your-dockerhub-username/flask-docker-demo:1.0`
- `docker pull your-dockerhub-username/flask-docker-demo:1.0`

### Cleanup

- `docker container prune`
- `docker image prune`
- `docker system prune -a`

## 9. Step-by-Step: Run This Project with Docker

From the repository root:

1. Build image:

```bash
docker build -t flask-docker-demo:1.0 -f dockerfile .
```

2. Start container:

```bash
docker run -d --name flask-demo -p 5000:5000 flask-docker-demo:1.0
```

3. Open app:

- Browser: `http://localhost:5000`

4. Check logs:

```bash
docker logs -f flask-demo
```

5. Stop and remove container:

```bash
docker stop flask-demo
docker rm flask-demo
```

## 10. Volumes and Data Persistence

Containers are ephemeral; deleted container means writable layer data is lost.

- Named volume: managed by Docker.
- Bind mount: host directory mounted into container.

Examples:

- Named volume: `docker run -v mydata:/data ...`
- Bind mount: `docker run -v ${PWD}:/app ...`

## 11. Docker Networking Theory

Default network driver: bridge.

- Container-to-container communication via network names.
- Port mapping (`-p host:container`) exposes container port to host.
- For multi-container apps, create custom network:

```bash
docker network create app-net
docker run -d --name service-a --network app-net image-a
docker run -d --name service-b --network app-net image-b
```

## 12. Docker Compose (Multi-Container)

Use Docker Compose when you have app + database + cache + other services.

Typical commands:

- `docker compose up -d`
- `docker compose ps`
- `docker compose logs -f`
- `docker compose down`

## 13. Security Basics

- Avoid running as root.
- Use trusted base images.
- Scan images (`docker scout` or third-party scanners).
- Keep dependencies and base images patched.
- Do not hardcode secrets in images.

## 14. Performance and Optimization

- Reduce image layers and size.
- Use multi-stage builds.
- Cache dependencies smartly.
- Avoid copying unnecessary files.
- Prefer deterministic builds and pinned versions.

## 15. Troubleshooting Quick Guide

- Port already in use: change host port mapping.
- Container exits immediately: inspect with `docker logs <container>`.
- Build fails at install: verify internet/proxy and dependency versions.
- File not found in image: verify `COPY` paths and build context.

## 16. Docker in CI/CD (Theory)

Common pipeline flow:

1. Run tests.
2. Build Docker image.
3. Tag with version + commit SHA.
4. Push to registry.
5. Deploy image to server/orchestrator.

## 17. Learning Roadmap

1. Master image/container basics.
2. Learn Compose deeply.
3. Practice image optimization and security scanning.
4. Explore orchestration (Kubernetes).
5. Build full CI/CD pipeline around Docker images.

---

## Quick Start Summary

```bash
docker build -t flask-docker-demo:1.0 -f dockerfile .
docker run -d --name flask-demo -p 5000:5000 flask-docker-demo:1.0
```

Open `http://localhost:5000` and test your Dockerized Flask app.
# Jenkins Docker

A local Jenkins environment for running CI/CD pipelines with Docker support.

## Overview

This project runs Jenkins inside a Docker container using Docker Compose.

Jenkins can access the host Docker daemon through the Docker socket:

```text
/var/run/docker.sock
```

This allows Jenkins pipelines to run Docker commands such as `docker build`, `docker run`, and `docker push`.

This setup uses the host Docker socket. It is not a full Docker-in-Docker setup.

## Project Structure

```text
jenkins-docker/
├── .gitignore
├── Dockerfile
├── README.md
├── docker-compose.yml
└── plugins.txt
```

## Features

- Jenkins LTS running in Docker
- Docker CLI available inside the Jenkins container
- Python 3, pip, and venv for Python-based pipelines
- curl for health checks
- Jenkins plugins installed from `plugins.txt`
- Persistent Jenkins data using a named Docker volume
- Access to the host Docker daemon through `/var/run/docker.sock`

## Prerequisites

- Docker
- Docker Compose

On macOS, Docker Desktop must be running before starting Jenkins.

## Start Jenkins

Build and start Jenkins:

```bash
docker compose up -d --build
```

Open Jenkins in the browser:

```text
http://localhost:8080
```

## Get the Initial Admin Password

```bash
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

Use this password to complete the initial Jenkins setup.

## Docker Access

The Jenkins container mounts the host Docker socket:

```yaml
- /var/run/docker.sock:/var/run/docker.sock
```

The container is also added to group `0`:

```yaml
group_add:
  - "0"
```

This works for this local Docker Desktop setup. On some Linux systems, the Docker socket group may be different and `group_add` may need to be adjusted.

To verify Docker access from inside the Jenkins container:

```bash
docker exec -it jenkins docker ps
```

If the command lists running containers, Jenkins can access Docker.

## Common Commands

Start Jenkins:

```bash
docker compose up -d
```

Rebuild and start Jenkins:

```bash
docker compose up -d --build
```

Stop Jenkins:

```bash
docker compose stop
```

Restart Jenkins:

```bash
docker compose restart
```

View logs:

```bash
docker compose logs -f
```

Remove the Jenkins container:

```bash
docker compose down
```

Remove the Jenkins container and delete Jenkins data:

```bash
docker compose down -v
```

## Usage Note

This setup mounts the host Docker socket so Jenkins can run Docker commands from inside the container.

It is designed for local development, CI/CD practice, and portfolio projects.

## Companion Project

This Jenkins environment can be used with the companion FastAPI project:

[python-app-jenkins](https://github.com/pouyaarjomandi/python-app-jenkins)

The companion project contains a FastAPI application and a Jenkins pipeline that runs tests, builds a Docker image, pushes it to Docker Hub, deploys the container, and verifies the `/health` endpoint.
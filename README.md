# jenkins-docker

Run Jenkins locally using Docker Compose with Docker-in-Docker support.

## Overview

This setup runs Jenkins in a Docker container with access to the host Docker daemon, allowing Jenkins pipelines to build and push Docker images.

## Project Structure

```
jenkins-docker/
├── Dockerfile          # Custom Jenkins image with Docker and Python
├── docker-compose.yml  # Jenkins service definition
├── jenkins_home/       # Jenkins data (ignored by git)
└── .gitignore
```

## Features

- Jenkins LTS running in Docker
- Docker CLI available inside Jenkins for pipeline builds
- Python 3 + pip available for running tests
- Bind mount for easy access to Jenkins data
- Health check configured
- Isolated Docker network

## Getting Started

### Prerequisites

- Docker + Docker Compose

### Setup

```bash
# Clone the repo
git clone https://github.com/pouyaarjomandi/jenkins-docker.git
cd jenkins-docker

# Create jenkins_home with correct ownership (macOS)
mkdir jenkins_home
sudo chown -R 501:20 jenkins_home

# Build and start Jenkins
docker compose up -d
```

Jenkins will be available at `http://localhost:8080`

### Get Initial Admin Password

```bash
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

## Useful Commands

| Task | Command |
|------|---------|
| Start | `docker compose up -d` |
| Stop | `docker compose stop` |
| Restart | `docker compose restart` |
| View logs | `docker compose logs -f` |
| Remove | `docker compose down` |

## Notes

- `jenkins_home/` is mounted as a bind mount so Jenkins data persists on your machine
- Docker socket is mounted to allow pipelines to build Docker images (DooD pattern)
- `group_add: "0"` gives the jenkins user access to the Docker socket without running as root

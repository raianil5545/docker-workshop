# Docker Commands Quick Reference

Common commands for working with Docker (same across macOS, Windows, and Ubuntu).

## Detached Mode vs Interactive Mode

| Command | What it does |
|---|---|
| `docker run -d <image>` | **Detached mode** — runs the container in the background and returns your terminal prompt immediately |
| `docker run -it <image>` | **Interactive mode** — attaches your terminal to the container's input/output, so you can interact with it live |
| `docker run -it <image> bash` | Interactive mode + opens a bash shell inside the container |
| `docker run -itd <image>` | Combines both — starts detached, but keeps STDIN open so you can attach later |
| `docker attach <container>` | Attach your terminal to an already-running (detached) container |
| `docker exec -it <container> bash` | Open a new interactive shell inside a container that's already running |
| `docker logs -f <container>` | Follow the logs of a detached container in real time (without attaching to it) |

**Flag breakdown:**
- `-d` / `--detach` — run in the background, print the container ID, and return control of the terminal
- `-i` / `--interactive` — keep STDIN open even if not attached, so you can type input
- `-t` / `--tty` — allocate a pseudo-terminal, giving you a proper interactive shell (colors, line editing, etc.)
- `-i` and `-t` are almost always used together as `-it`

**When to use which:**
- Use **detached (`-d`)** for things like web servers, databases, or background services you don't need to interact with directly
- Use **interactive (`-it`)** when you want a live shell — debugging, running one-off scripts, or exploring an image

Example — run an Nginx server in the background, then jump into it interactively:
```bash
docker run -d --name web nginx
docker exec -it web bash
```

## Containers

| Command | What it does |
|---|---|
| `docker ps` | List running containers |
| `docker ps -a` | List all containers (including stopped) |
| `docker run <image>` | Start a container from an image |
| `docker run -d <image>` | Start a container in detached (background) mode |
| `docker run -it <image> bash` | Start a container and open an interactive shell |
| `docker stop <container>` | Stop a running container |
| `docker start <container>` | Start a stopped container |
| `docker restart <container>` | Restart a container |
| `docker rm <container>` | Remove a stopped container |
| `docker logs <container>` | View a container's logs |
| `docker exec -it <container> bash` | Open a shell inside a running container |

## Images

| Command | What it does |
|---|---|
| `docker images` | List downloaded images |
| `docker pull <image>` | Download an image from a registry |
| `docker build -t <name> .` | Build an image from a Dockerfile in the current directory |
| `docker rmi <image>` | Remove an image |
| `docker tag <image> <new-name>` | Tag an image with a new name |
| `docker push <image>` | Push an image to a registry |

## Docker Compose

| Command | What it does |
|---|---|
| `docker compose up` | Start services defined in `docker-compose.yml` |
| `docker compose up -d` | Start services in detached mode |
| `docker compose down` | Stop and remove those services |
| `docker compose ps` | List services and their status |
| `docker compose logs` | View logs for all services |
| `docker compose build` | Build/rebuild service images |

## Cleanup

| Command | What it does |
|---|---|
| `docker container prune` | Remove all stopped containers |
| `docker image prune` | Remove unused images |
| `docker volume prune` | Remove unused volumes |
| `docker system prune` | Remove all unused containers, images, and networks |
| `docker system prune -a` | Same as above, but also removes unused images not tied to a container |

## Info & Inspection

| Command | What it does |
|---|---|
| `docker --version` | Show Docker version |
| `docker compose version` | Show Docker Compose version |
| `docker info` | Show system-wide Docker info |
| `docker inspect <container/image>` | Show detailed JSON info about a container or image |
| `docker stats` | Live resource usage stats for running containers |
## Docker Image – Universal App Packaging

- Dockerfile:
  - FROM
  - RUN
  - WORKDIR
  - COPY
  - CMD
  - (application distributed as tarball if needed)

- Image layers:
  - Python binaries + system libraries
  - Flask
  - Application code

- Build image:
  - docker build -t name:tag .


# Docker Registry – Universal App Distribution (e.g. Docker Hub)

- docker push user/repo:tag
- docker pull user/repo:tag

TIP:
https://labs.play-with-docker.com/


# Docker Container – Identical Runtime Environments

- Container = running instance of image
- Provides isolation
- Reduces environment variance
- Increases speed of change


# Docker Installation (Ubuntu Server)

- sudo apt install docker.io
- docker run hello-world
- docker login
- export DOCKER_HOST=tcp://<ip>:2375


# Monitoring & Inspection

- docker ps
- docker top <container>
- docker stats
- docker inspect <container>


# Working with Containers

- docker container run -it <image> [options]
- docker container exec -it <container> bash
- docker start -ai <container_name>
- docker image ls


# Docker Networking

- docker container port <container>
- docker network ls
- docker network inspect <network>

- docker network create --driver bridge <network_name>
- docker network connect <network_name> <container>
- docker network disconnect <network_name> <container>

- docker network prune
- docker network rm <network_name>


# Docker Networks DNS

- Built-in DNS
- Containers in the same network communicate using container names


# Docker Hub

- docker pull name:version
- docker login
- docker logout
- docker push user/repo:tag


# Docker Images & Storage

- docker image ls
- docker image history <image>

- Image layers
- Storage drivers
- Docker volumes vs bind mounts


# Dockerfile Core Instructions

- FROM
- RUN
- EXPOSE
- COPY
- WORKDIR
- CMD


# Core Concepts

- Immutable infrastructure
- Ephemeral containers
- Data volumes
- Bind mounts
- Separation of concerns


# Data Volumes

- Volumes (creating without /)
  - docker volume ls
  - docker volume inspect <name_volume>
  - docker run -v name_of_data:/name_of_mout_data ex. /var/lib/name
  - VOLUME /data   (inside Dockerfile)
  - docker volume create <volume_name>
  - docker run -v <volume_name_without_/>:/data <image>
  - searching name of volume in container docker inspect name_of_container | grep Mounts -A 15 (print 15 lines after target)
  - docker volume create --help
  - docker volume create

- Bind Mount (always with /)
  - docker run -v $(pwd):/path_to_the_container_files

## Dockerfile

# Buildtime vs. Runtime

| Instruction | When | Type |
|---|---|---|
| `FROM` | Buildtime | Additive |
| `ADD` | Buildtime | Additive |
| `COPY` | Buildtime | Additive |
| `RUN` | Buildtime | Additive |
| `ONBUILD` | Buildtime | Additive |
| `ARG` | Buildtime | Overwrite |
| `LABEL` | Build + Runtime | Overwrite |
| `ENV` | Build + Runtime | Overwrite |
| `SHELL` | Build + Runtime | Overwrite |
| `USER` | Build + Runtime | Overwrite |
| `WORKDIR` | Build + Runtime | Overwrite |
| `EXPOSE` | Runtime | Additive |
| `VOLUME` | Runtime | Additive |
| `CMD` | Runtime | Overwrite |
| `ENTRYPOINT` | Runtime | Overwrite |
| `STOPSIGNAL` | Runtime | Overwrite |
| `HEALTHCHECK` | Runtime | Overwrite |

Additive - can't be overwritten with a second statement.

Overwrite - replace their previous use.

# Linux Signals & PID 1 in Docker

## Signals

| Signal | Description |
|------|------|
| SIGTERM | Graceful shutdown |
| SIGINT | Interrupt (Ctrl+C) |
| SIGKILL | Immediate kill |

Example:


kill -15 <PID>   # SIGTERM
kill -9 <PID>    # SIGKILL


## Docker Stop Behavior


docker stop <container>


Docker sends:


SIGTERM → wait 10s → SIGKILL


## PID 1 in Containers

PID 1 is the main process in a container.

Responsibilities:
- Handle signals
- Clean zombie processes

Problem:
Most apps (Node, Python, etc.) are not designed to run as PID 1


## Solution

dockerfile
ENTRYPOINT ["/sbin/tini", "--"]
CMD ["node", "app.js"]


Or run container with:

docker run --init <image>

# Shell Form vs Exec Form
## The Two Forms

dockerfile
# Exec form — JSON array, launched directly
ENTRYPOINT ["node", "server.js"]

# Shell form — plain string, launched via /bin/sh
ENTRYPOINT node server.js


---

## Key Difference — PID 1

Shell form — Docker launches `/bin/sh -c "node server.js"`. The shell is PID 1, your app is PID 2 (a child process). Signals from `docker stop` go to the shell, which ignores them. After 10 seconds Docker sends SIGKILL — brutal kill, no graceful shutdown.


docker stop → SIGTERM → /bin/sh (PID 1) → ignored → SIGKILL after 10s 


Exec form — Docker launches `node` directly. Your app is PID 1. Signals reach it immediately and it can shut down gracefully.


docker stop → SIGTERM → node (PID 1) → graceful shutdown ✅


---

## When to Use Each

| Instruction | Form | Reason |
|---|---|---|
| `RUN` | shell | needs `&&`, variables, pipes during build |
| `ENTRYPOINT` | exec | app must be PID 1 to receive signals |
| `CMD` | exec | appended to ENTRYPOINT as a clean argument |

---

## Golden Rule

Always use exec form in `ENTRYPOINT` and `CMD`.
Always use shell form in `RUN`.
Never mix forms between `ENTRYPOINT` and `CMD`.


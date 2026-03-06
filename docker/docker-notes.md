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

| Instrukcja | Kiedy działa | Typ |
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
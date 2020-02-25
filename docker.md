# Docker

![](.gitbook/assets/image%20%288%29.png)

## Commands

#### docker-compose

Automate building container images and the deployment of multi-container applications

```bash
docker-compose up -d
```

#### docker images

See the created images

```bash
$ docker images

REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

#### docker ps

see the running containers

```bash
$ docker ps

CONTAINER ID        IMAGE             COMMAND                  CREATED             STATUS              PORTS                           NAMES
82411933e8f9        azure-vote-front  "/usr/bin/supervisord"   57 seconds ago      Up 30 seconds       443/tcp, 0.0.0.0:8080->80/tcp   azure-vote-front
b68fed4b66b6        redis             "docker-entrypoint..."   57 seconds ago      Up 30 seconds       0.0.0.0:6379->6379/tcp          azure-vote-back
```

#### docker-compose down

Stop and remove the container instances and resources

```text
$ docker-compose down
Stopping azure-vote-front ... done
Stopping azure-vote-back  ... done
Removing azure-vote-front ... done
Removing azure-vote-back  ... done
```


## Access services running on host machine from inside a container service as `localhost`
1. Use --net="host" in your docker run command, then localhost in your docker container will point to your docker host.
    ```
    docker run -d --net="host" --restart=always --name=ltimdb_etl etl-postgres-neo4j-service:0.1
    ```
2. If you are trying to access services running on host machine from inside `docker-compose` managed container then all you need to do is to access service at `host.docker.internal`. No other configuration is needed.

    **Refer**: https://docs.docker.com/desktop/mac/networking/

## Build a docker image
```sh
# Change to directory that has Dockerfile
$ docker build --tag codingkapoor/dropbox:latest .
```

## Delete docker volumes that are not being used by any container
```sh
# List all the docker volumes
$ docker volume ls

# Delete all docker volumes that are not being used by any container
$ docker volume prune
```

Data written persists even if you stop the container. If however you wish to rid volume data as soon as you exit container use `--rm` arg to `docker run` command like so:
```sh
$ docker run --rm -i -v=named-data-volume:/tmp/ghost busybox find /tmp/ghost
```

**Note**: This will also rid the container created from the image. In other words, container won't be listed under `docker ps -a`.

## Enable Buildkit
Buildkit provides a great set of features on top of docker. One of it being facilitation of building images faster by employing caching, parallelization and skipping non-target stages in a multi-stage build. When using `docker-compose` export following environment variables to enable buildkit.
```
$ export DOCKER_BUILDKIT=1
$ export COMPOSE_DOCKER_CLI_BUILD=1
```

Refer this [blog](https://pythonspeed.com/articles/docker-buildkit/).


## Follow container logs
```sh
$ docker logs -f mywebsite_dropbox-ghost-technology_1
```

## List docker volume data
The data present in a docker volume could be accessed like so:
```sh
$ sudo ls /var/lib/docker/volumes/eb8584883bb7c9894d0f8a8363770d8089d44a78f36720b129250146046a97de/_data
$ cat ls /var/lib/docker/volumes/eb8584883bb7c9894d0f8a8363770d8089d44a78f36720b129250146046a97de/_data/stdout.log 
```

## Multi-stage based builder pattern to create docker images
- To create a reactjs docker image
```dockerfile
FROM node:16-alpine AS base
WORKDIR /app
COPY . .

FROM base AS development
ENV NODE_ENV development
RUN npm install
EXPOSE 3000
CMD [ "npm", "start" ]

FROM base AS builder
ENV NODE_ENV production
RUN npm install --production
RUN npm run build

# Bundle static assets with nginx
FROM nginx:1.21.0-alpine AS production
ENV NODE_ENV production
COPY --from=builder /app/build /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

- To create nodejs docker image
```dockerfile
FROM node:16-alpine AS base
WORKDIR /app
COPY . .

FROM base AS development
ENV NODE_ENV development
RUN npm install
EXPOSE 4000
CMD [ "npm", "start" ]

FROM base AS builder
ENV NODE_ENV production
RUN npm install --production
RUN npm run build

FROM node:16-alpine AS production
WORKDIR /app
ENV NODE_ENV production
COPY --from=builder /app/build /app
COPY --from=builder /app/public /app/public
EXPOSE 80
CMD [ "node", "./index.js" ]
```

## Run a container

```sh
$ docker run -d --restart=always --name=dropbox janeczku/dropbox
$ docker exec -it busybox /bin/bash

# as a root user
$ docker exec -u 0 -it ltimdb_etl /bin/bash
```

## Set `path` in Dockerfile
Trying to set PATH via RUN command as `RUN . ~/.bashrc` doesn't seem to work. Docker provides `ENV` command to achieve the same as shown below:
```
ENV PATH="/opt/gtk/bin:${PATH}"
```

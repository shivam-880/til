## Access services running on host machine from inside a container service as `localhost`
Use --net="host" in your docker run command, then localhost in your docker container will point to your docker host.
```
docker run -d --net="host" --restart=always --name=ltimdb_etl etl-postgres-neo4j-service:0.1
```

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

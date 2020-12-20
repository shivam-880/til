## Build a docker image

```sh
// change to directory that has Dockerfile

$ docker build --tag codingkapoor/dropbox:latest .
```

## Check the content of a docker volume

``` bash
$ docker run --rm -i -v=named-data-volume:/tmp/ghost busybox find /tmp/ghost
```

## Delete docker volumes that are not being used by any container

```sh
// List all the docker volumes

$ docker volume ls


// Delete all docker volumes that are not being used by any container

$ docker volume prune
```

# Follow container logs

```sh
$ docker logs -f mywebsite_dropbox-ghost-technology_1
```

# Run a container

```sh
docker run -d --restart=always --name=dropbox janeczku/dropbox
docker exec -it busybox /bin/bash
```

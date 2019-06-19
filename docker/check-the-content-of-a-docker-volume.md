# Check the content of a docker volume

```sh
docker run --rm -i -v=named-data-volume:/tmp/ghost busybox find /tmp/ghost
```

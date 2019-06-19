# Check the content of a docker volume

``` bash
$ docker run --rm -i -v=named-data-volume:/tmp/ghost busybox find /tmp/ghost
```

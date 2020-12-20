## Determine if a port is already in use

#### Cmd
```
netstat -nlp | grep 7199
```

#### Code
```
if lsof -Pi :8080 -sTCP:LISTEN -t >/dev/null ; then
    echo "Port already in use!"
    exit 0 ;
fi
```

## Lines of code

```
find . -type f -exec wc -l {} +
find . -type f -exec cat {} + | wc -l
```

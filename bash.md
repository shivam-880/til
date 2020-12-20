# Determine If Port Is Already In Use

### CMD
```
netstat -nlp | grep 7199
```

### Programmatically
```
if lsof -Pi :8080 -sTCP:LISTEN -t >/dev/null ; then
    echo "Port already in use!"
    exit 0 ;
fi
```

# Lines Of Code

```
find . -type f -exec wc -l {} +
find . -type f -exec cat {} + | wc -l
```

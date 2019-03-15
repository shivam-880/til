# Determine If Port Is Already In Use

```
if lsof -Pi :8080 -sTCP:LISTEN -t >/dev/null ; then
    echo "Port already in use!"
    exit 0 ;
fi
```

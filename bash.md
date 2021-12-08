## Extract IP Address
```
ADDR=`sudo ip addr show docker0 | grep inet | grep -v inet6 | awk '{print $2}' | cut -d'/' -f1`
echo $ADDR
```

## Configure Base Dir
This will return the absolute path of the script itself. You could find other directories relative to this path.
```
BASEDIR="$( cd -- "$(dirname "$0")" >/dev/null 2>&1 ; pwd -P )"
echo $BASEDIR
```

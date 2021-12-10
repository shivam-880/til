## Banners in Bash Scripts
Refer http://patorjk.com/software/taag

## Configure Base Dir
This will return the absolute path of the script itself. You could find other directories relative to this path.
```bash
BASEDIR="$( cd -- "$(dirname "$0")" >/dev/null 2>&1 ; pwd -P )"
echo $BASEDIR
```

## Extract IP Address
```bash
ADDR=`sudo ip addr show docker0 | grep inet | grep -v inet6 | awk '{print $2}' | cut -d'/' -f1`
echo $ADDR
```

## Interactive Bash Script
```bash
while true; do
  read -p 'Provision Cassandra? (y/n): ' pcass
  case $pcass in
    [Yy]* ) ./cass/scripts/provision.sh; break;;
    * ) echo 'Skipped!'; break;;
  esac
done
```

Or,
```bash
while true; do
  read -p 'do you want to continue "y" or "n": ' yn
  case $yn in
    [Yy]* ) echo 'this program continue '; break;;
    [Nn]* ) exit;;
    * ) echo 'Please answer yes or no: ';;
  esac
done
```

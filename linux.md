## Create tunnel
```sh
$ ssh -f -N -L 8989:localhost:8981 bst-03
```

## Determine if a port is already in use

#### Cmd
```sh
$ netstat -nlp | grep 7199
```

## Determine if a port is open on a remote server
```
$ nc -v neo4j 7474
```

#### Code
```sh
if lsof -Pi :8080 -sTCP:LISTEN -t >/dev/null ; then
    echo "Port already in use!"
    exit 0 ;
fi
```

## How to create windows bootable disk on ubuntu

1. Download `woeusb`. Refer: https://github.com/slacka/WoeUSB/issues/311
```sh
$ wget https://github.com/slacka/WoeUSB/files/4622440/woeusb.zip
```

2. Extract and install `woeusb`
```sh
$ sudo apt install ./woeusb_3.3.1_amd64.deb
$ sudo apt install ./woeusb-dbgsym_3.3.1_amd64.deb
```

3. Format usb drive with NTFS format and make sure it is unmounted
```sh
$ sudo umount /dev/sdd1
```

4. Create bootable
```sh
$ sudo woeusb --target-filesystem NTFS --device Windows_10_Pro_en-US_v1909_x64_BiT_Activated.iso /dev/sdd
```

## How to format bootable ubuntu usb drive
1. Download and install `gparted`
```sh
$ sudo apt-get update && sudo apt-get install gparted -y 
```

2. Choose your drive and format

## Install necessary tools
```
$ apt-get update && apt-get install -y netcat
$ apt-get update && apt-get install -y net-tools
$ apt-get update && apt-get install -y iputils-ping
```

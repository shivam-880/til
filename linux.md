## Create tunnel
```sh
$ ssh -f -N -L 8989:localhost:8981 bst-03
$ ssh -i aws.pem -N -f -L 4000:localhost:3000 ubuntu@ec2-10-2-199-58.compute-1.amazonaws.com

# ssh -f -N -L <local ipaddr>:<local port>:<destination ipaddr>:<destination port> <remote ipaddr>
```

## Determine if a port is already in use
```sh
$ netstat -nlp | grep 7199  # Or
$ lsof -n -i4TCP:8000 | grep LISTEN # mac
```

## Determine if a port is open on a remote server
```
$ nc -v neo4j 7474
```

```sh
# code
if lsof -Pi :8080 -sTCP:LISTEN -t >/dev/null ; then
    echo "Port already in use!"
    exit 0 ;
fi
```

## Grep in Parallel
Refer: https://stackoverflow.com/questions/11898949/how-do-i-grep-in-parallel

```sh
$ find . -type f | parallel -k -j150% -n 1000 -m grep -H -n "2022-02-28" {} | grep "2022-02-28 23:49" | grep -v "2022-02-28 23:49:12"
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

## Keyboard shortcut to force exit ssh shell
```
ENTER~.
```

## Using `rsync` for data transfer
- Using `rsync`
    ```sh
    rsync -e "ssh -i /home/ubuntu/.ssh/mykeys/aws.pem" -r /home/ubuntu/data ubuntu@172.31.89.208:/home/ubuntu/data;
    ```

- Using `rsyncd`. [Avoids encryption](https://www.upguard.com/blog/secure-rsync#:~:text=However%2C%20the%20rsync%20daemon%20does,extremely%20vulnerable%20to%20data%20exposure.)

    Refer: https://serverspace.io/support/help/use-rsync-to-create-a-backup-on-ubuntu/

## Update AWS IP in your local dns
1. Make aws host entry in `~/.ssh/config`
    ```
    Host aws
        HostName aws
        User ubuntu
        IdentityFile ~/.ssh/mykeys/aws.pem
    ```
2. Add domain name ip addr mapping in `/etc/hosts`
    ```
    44.201.182.31 aws
    ```

3. Add a function `~/.zshrc` to update ip in `/etc/hosts`
    ```
    function changedns(){ sudo sed -i -e "s/.*aws.*/$1 aws/"  /etc/hosts;}
    ```

4. Usage
    ```
    $ changedns 44.21.12.33
    $ ssh aws
    ```

## How to create windows bootable disk on ubuntu

1. Download `woeusb`. Refer: https://github.com/slacka/WoeUSB/issues/311
```
wget https://github.com/slacka/WoeUSB/files/4622440/woeusb.zip
```

2. Extract and install `woeusb`
```
sudo apt install ./woeusb_3.3.1_amd64.deb
sudo apt install ./woeusb-dbgsym_3.3.1_amd64.deb
```

3. Format usb drive with NTFS format and make sure it is unmounted
```
sudo umount /dev/sdd1
```

4. Create bootable
```
sudo woeusb --target-filesystem NTFS --device Windows_10_Pro_en-US_v1909_x64_BiT_Activated.iso /dev/sdd
```

## How to format bootable ubuntu usb drive
1. Download and install gparted
```
sudo apt-get update && sudo apt-get install gparted -y 
```

2. Choose your drive and format

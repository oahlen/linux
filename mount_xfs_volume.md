# Mount XFS volume

## Find the disk
```
lsblk -o NAME,HCTL,SIZE,MOUNTPOINT | grep -i "sd"
````

## Format disk (sda) as XFS
```
sudo parted /dev/sda --script mklabel gpt mkpart xfspart xfs 0% 100%
sudo mkfs.xfs /dev/sda1
sudo partprobe /dev/sda1
```

## Create data volume
```
sudo mkdir /data
sudo mount /dev/sda1 /data
```

## Find UUID of disk
```
sudo blkid
```

## Add entry in fstab (UUID from earlier)
```
UUID={UUID}   /data   xfs   defaults,nofail   1   2
```

## Verify the disk (should be mounted at /data)
```
lsblk -o NAME,HCTL,SIZE,MOUNTPOINT | grep -i "sd"
```

## Run fstrim daily

```
sudo apt-get install util-linux
```

Run `sudo crontab -e` and insert the following line:

```
@daily root /sbin/fstrim /data
```

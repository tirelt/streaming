# Local streaming

Local streaming stack

All credits to [zerodya](https://zerodya.net/self-host-jellyfin-media-streaming-stack/), we are just following his tutorial and adding useful tips for our setup.

DISCLAIMER: This article serves educational purposes only.

## Stack control / docker

### Docker install

We installed `docker engine`  and `docker compose` plugin following the official [docs](https://docs.docker.com/engine/install/debian/) using the `apt` repository.

### Controls

We use the new version of `docker compose` so the commands are (in the folder with the `docker-compose.yml` file)
```Bash
sudo docker compose up -d # to create and update the stack, need to run for both the streaming and downloading stacks
sudo docker compose stop # to stop all the contianers but not remove them (down would remove)
sudo docker compose start # to restart the containers
```
Alternativly we can directly manage the containers indivually 
```Bash
sudo docker ps # to check the running containers, -a all the conatiners, -q shows only the ids
docker stop <container_ID> # to stop a container
docker start <container_ID># to start a container 
```
## Using external drive on Linux

```Bash
sudo fdisk -l # to get the list of drive
sudo fdisk /dev/sdX # to manage partition on the disk /dev/sdX
```
It opens th manager, `m` for help, `n` for new partition, `g` to create a new empty partition table and erase anything on the disk. We opted to create a new table with only 1 partition (so we selected defualt partition number, first and last sector).
Finallay `w` to write.
```Bash
sudo mkfs -t ext4 /dev/sdX1 # to add a file system to our partition sdX1. We chose ext4 for Linux
sudo mkdir -p /media/ssd # to create a directory
sudo mount /dev/sdX1 /media/ssd # to mount the partition sdX1 
```
To safely remove let's stop the stack and do 
```Bash
lsof /media/ssd # to check the usage
sudo umount /media/ssd # to unmount
sudo eject /dev/sdX # to eject 
```
To reconnect simply remount 
```Bash
sudo mount /dev/sdX1 /media/ssd
```

## Tips 
- check the firewall to open necessary ports
- 

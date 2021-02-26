#### 언마운트

umount /dev/sdb1



#### 장치보기

lsblk



#### 마운트된 장치 보기

df -h



#### 포맷하기

parted /dev/sdb

parted) mklabel msdos

Yes

parted) mkpart primary fat32 1MiB 100%

parted) set 1 boot on

parted)  quit



mkfs.vfat /dev/sdb1



#### 마운트

mount /dev/sdb /mnt/my


# ทำการอัพเดตระบบและติดตั้ง mdadm
`pt update && sudo apt -y full-upgrade && apt install -y mdadm`
# ใช้คำสั้ง lsblk เพื่อตรวจสอบ disk
ตัวอย่าง
```
~$ lsblk
$NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
$sda           8:0    0 931.5G  0 disk
$sdb           8:16   0 931.5G  0 disk
$nvme1n1     259:0    0 238.5G  0 disk
$nvme0n1     259:1    0 232.9G  0 disk
$├─nvme0n1p1 259:2    0   512M  0 part /boot/efi
$├─nvme0n1p2 259:3    0 231.4G  0 part /
$└─nvme0n1p3 259:4    0   977M  0 part [SWAP]
```
# สร้าง RAID 1
ในกรณีนี้การสร้าง raid จะใช้ disk ชื่อ sda และ sdb ซึ่งมีขนาดเดียวกันและจะใช้สร้าง RAID 1
`sudo mdadm --create /dev/md0 --raid-devices=2 --level=1 /dev/sda /dev/sdb`
# ตรวจสอบสถานะของ RAID
`cat /proc/mdstat`
ตัวอย่าง
```
$ cat /proc/mdstat
Personalities : [raid1]
md0 : active raid1 sdb[1] sda[0]
      976630464 blocks super 1.2 [2/2] [UU]
      bitmap: 0/8 pages [0KB], 65536KB chunk

unused devices: <none>
```
# เพิ่ม RAID array ลงใน /etc/fstab
`sudo blkid /dev/md0`
`UUID=<UUID>  /mnt/raid1  ext4  defaults  0  0`
# ทำการเริ่มระบบใหม่
`reboot`

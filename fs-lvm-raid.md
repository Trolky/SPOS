# Práce s FS

Užitečné příkazy

```sh
df -h
mount
mount -a
mount /dev/sda1 /mnt/
```

Oddíly

```sh
fdisk -l /dev/sda
cfdisk /dev/sda
sfdisk -s
```

Práce s file systémy

```sh
mkfs.ext4 /dev/sda1
mkfs.xfs /dev/sda2
mount /dev/sda2 /mnt/

resize2fs /dev/sda1
xfs_grow /dev/sda2

mkswap /dev/sda3
swapon /dev/sda3
```

Práce s obrazy

```sh
dd if=/dev/zero of=/tmp/image bs=1M count=1024

losetup -f /tmp/image
mkfs.ext4 /dev/loop0
mount /dev/loop0 /mnt/
```

Automatické připojení po startu

```
# /etc/fstab
/dev/data/public-share	/opt/share	ext4	defaults	0	2
LABEL=share				/home		ext4	defaults	0	2
10.0.2.15:/opt/share	/share		nfs		defaults	0	2
```

# Raid

Druhy raidu
- **raid0** - linear / striping
- **raid1** - mirroring
- **raid5** - striping + parita (3 disky)
- **raid6** - striping + 2x parita (5 disků)

```sh
apt install mdadm
```

Tvorba raidu

```sh
mdadm --create --verbose /dev/md0 --level=1 --raid-devices=2 /dev/vdb1 /dev/vdc1
mdadm -Cv /dev/md0 -l1 -n2 /dev/vd[bc]1
mdadm -Cv /dev/md1 -l5 -n3 /dev/vdb1 /dev/vdc1 missing
```

Stav raidu

```sh
cat /proc/mdstat
mdadm --detail /dev/md0
```

Práce s raidem

```sh
mdadm --fail /dev/md0 /dev/vdb1
mdadm --remove /dev/md0 /dev/vdb1
mdadm --add /dev/md0 /dev/vdd1
```

Zrušení raidu

```sh
mdadm --stop /dev/md0
mdadm --remove /dev/md0
mdadm --zero-superblock /dev/vdb1
```

Zvětšení počtu disků

```sh
mdadm --grow /dev/md0 -n3
```

# LVM

```sh
apt install lvm2
```

Pojmy
- **PV**: Physical Volume - fyzický disk
- **VG**: Volume Group - skupina fyzických disků
- **LV**: Logical Volume - logický disk

Tvorba a práce s PV

```sh
pvcreate /dev/vdd1
pvmove -v /dev/sda2 /dev/sdb2
```
- další příkazy: `pvdisplay`, `pvscan`, `pvremove`

Tvorba a práce s VG

```sh
vgcreate vg-data /dev/vdd1
vgextend vg-data /dev/vdc1
vgreduce vg-data /dev/vdd1
```
- další příkazy: `vgdisplay`, `vgscan`, `vgchange`, `vgremove`

Tvorba a práce s LV

```sh
lvcreate -L 12M -n moje-data vg-data
lvextend -L 24M /dev/vg-data/moje-data
```
- další příkazy: `lvdisplay`, `lvscan`, `lvchange`, `lvresize`, `lvrename`

Snapshoty

```sh
lvcreate --size 100M --snapshot -n moje-data-snap /dev/vg-data/moje-data
lvremove /dev/vg-data/moje-data-snap
```

Změna velikosti FS uvnitř LV

```sh
resize2fs /dev/vg-data/moje-data
```

## LINUX VOLUME MANAGEMENT 
**Linux storage has layers.**

## CORE CONCEPTS    

### Physical Disk
examples:
- `nvme0n1` - NVMe disk
- `vda` - VirtIO disk
- `sda` - SATA/SCSI disk

This is the raw block devices.

- `lsblk` - to list all the blocks
- `df -h` - to get mount points and storage



### Partitions
```
ubuntu@ip-172-31-31-249:~$ lsblk
NAME         MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
loop0          7:0    0 27.6M  1 loop /snap/amazon-ssm-agent/11797
loop1          7:1    0   74M  1 loop /snap/core22/2163
loop2          7:2    0 50.9M  1 loop /snap/snapd/25577
nvme0n1      259:0    0    8G  0 disk      #Partitions
├─nvme0n1p1  259:1    0    7G  0 part /
├─nvme0n1p14 259:2    0    4M  0 part 
├─nvme0n1p15 259:3    0  106M  0 part /boot/efi
└─nvme0n1p16 259:4    0  913M  0 part /boot
```
Partitions are optional but but recommended for clarity.



### LVM(Logical Volume Manager)
LVM gives linux flexibility superpower
`Disk → PV → VG → LV → Filesystem → Mount`

Why LVM is used :
- Resize disk without downtime
- combine multiple disks


### Attached vs Mounted (Important Difference)

Attached -	Disk is connected to VM (cloud level)
Mounted	- Disk/LV is usable at a directory (OS level)

Attached ≠ usable
Mounted = usable

On Amazon Web Services, EBS volumes are attached, Linux volumes are mounted.


### LVM Commands
`pvs`/ `vgs`/ `lvs` - display summary of physical volume/ volume group/ logical volume.
`pvdisplay`/ `vgdisplay`/ `lvdisplay` - Detailed info of physical volume/ volume group/ logical volume.

## Steps to mount LV

Scenario
New disk attached: /dev/nvme1n1
Mount final storage at: /data

- Check disk
```
lsblk
```

- Create Physical Volume (PV) -attach volume first
```
pvcreate /dev/nvme1n1
```
    Disk is now under LVM control

- Create Volume Group (VG)
```
vgcreate data_vg /dev/nvme1n1
```

- Create Logical Volume (LV)
```
lvcreate -L 10G -n data_lv data_vg
```
    Resulting path:
    `/dev/data_vg/data_lv`

- Create filesystem on LV
```
mkfs.ext4 /dev/data_vg/data_lv
```



**- mkfs must be run on the logical volume because PVs and VGs contain LVM metadata and are not final block devices. Running mkfs on them would destroy LVM structures.”
“ext4 is a journaling Linux filesystem that defines how files are stored, organized, and recovered on a disk or logical volume.**


**- Critical Rule: Never format or mount a disk directly on / unless you are installing the OS. because Mounting on / means you overwrite the root filesystem, system files become hidden, OS may not boot, Package manager breaks, Logs vanish**

- Create mount point
```
mkdir /data
```  

- Mount LV
```
mount /dev/data_vg/data_lv /data
```

- Verify:
```
df -h
```


- To extend storage of lv
```
lvextend -L +5G /dev/data_vg/data_lv
lvextend -l +100%FREE /dev/data_vg/data_lv
``` 

-  To reduce size
```
umount /data
e2fsck -f /dev/data_vg/data_lv
resize2fs /dev/data_vg/data_lv 5G
lvreduce -L 5G /dev/data_vg/data_lv
mount /dev/data_vg/data_lv /data
df -h
``` 


## Mount Disk Without LVM

Used when:
-Simple setups
-Temporary storage
-No resizing needed

- Check disk
```
lsblk
```

- Create Physical Volume (PV) -attach volume first
```
pvcreate /dev/nvme3n1
```

- Create partition (Recommended)
```
fdisk /dev/nvme3n1
```

Inside fdisk:
```
n → p → 1 → enter → enter → w

#/dev/nvme3n1p1
```

- Create filesystem on partition
```
mkfs.ext4 /dev/nvme3n1p1
```

- Create mount point
```
mkdir /data
```

- Mount disk
```
mount /dev/nvme3n1p1 /data
```

Verify:
```
df -h
```

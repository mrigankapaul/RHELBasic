# Storage
## Disk and Partition
- Local Storage - Disk Drive and Solid State Drive<br>
- Network Storage - NAS and SAN <br>
- 2 major types of device file 
    - Character device file 
    - Block storage device file. 
- Block storage supports random access.
- Each device file has a major device number and a minor device number. 
    - Major - Device Type 
    - Minor - Device Instance
- All disk devices in linux has a universally unique id
### Partition types 
    - Primary Partition
    - Extended Partition
### Partition Scheme 
    - MBR Partition scheme
    - GPT Partition Scheme

### Useful Commands of storage
```
tty
echo "See I m not a file." > /dev/pts/0 // communicating with your terminal.
ls -l /dev | grep sd // to identify the SCSI devices or disk devices. sata or sas
fdisk -c /dev/sda // with option of 'p' for partition
parted // then select 'p
tail /var/log/messages // to check device driver activitie
```

### ssh to RHEL on Virtual Box
- select 'Network Adapter' -> Bridged Adapter (Attached  to) <br>
- renew IP Address loan on RHEL VM
```
sudo dhclient -r
sudo dhclient
ifconfig
```

### Partitioning Commands
```
// To create a MBR Partition Scheme
fdisk -c /dev/sdb
partprobe // forces the kernel to re-read the partition.
parted /dev/sdb // then select 'p'
select 't' // to change the partition type
select 'd' // to delete a partition

// To create a GPT Partition Scheme
parted /dev/sdb
mklabel gpt
select 'p' // to verify gpt partition scheme
unit MB
mkpart primary 0MB 50MB
rm 1 to delete the partition // we created partition 1 previous step
```

## File Systems
- Each partion gets its own filesystem and can be of different types.
- Any access to files in a file system, you need access to superblock ( Metatdata about file system)
- inode - Metadata about a file
- Deleting a fs is to either delete the partition or volume
```
cat /proc/filesystems // to list the supported file system and currently in use.
mke2fs -t ext4  /dev/sdb1 // creating a file system. -L // to assign a label
// Manual Mounting
mkdir /testmount
mount -t ext4 /dev/sdb1 /testmount
mount -l -t ext4 // to list all mounts for ext4 file system
umount /testmount // unmounting   // when you unmount it then we lose access to the file. However, if we again mount it to different path, we still have access to the files.
mount -t ext4 -o discard /dev/sdb1 /someothermount/
/dev/sdb1 /testmount ext4 ro,discard 0 0 // automatically mount by creating an entry in /etc/fstab file.
blkid // to get the uuid of the filesystem
lsblk --fs
UUID=c530ef6e-87f3-426c-8138-ff8722e63ab7 /testmount ext4 ro,discard 0 0 // create an entry in /etc/fstab file
mount -a // to avoid system reboot
umount /dev/sdb1 // either unmount the device or directory which is path to the mount.
lsof /testmount/
fuser -cuv /testmount/
umount -l /testmount/ // to unmount lazily
dumpe2fs /dev/sdb1 | grep -i superblock // to find the superblocks
ls -i /etc/ /// to get the inode of a file.
blkid // to check the label of a fs
e2label /dev/sdb1 "filesystem test" // to assign a label to a file system
LABEL="filesystem test" /testmount ext4 ro,discard 0 0 // create an entry in /etc/fstab file
fsck /dev/sdb1 // file system check on a device file
df -h /dev/sdb1 // information about the device file
df -hT // all the file system
du -h /etc/fstab / size of the file
du -hcs /etc/ summary of the size of /etc directory
```

### Linux LVM (Logical Volume Manager)
- LVM - Abstraction and Virtualization
- Physical Volumes - Volume Groups (Expanded or Contracted) - Logical Volumes (Expanded or Contracted)
- You can create Physical Volumes without any metadata as the metadata has info about the volume group and volume group can function with any one of the volumes in the group having the required metadata (Not Recommended)
- Recommendation - Single Partition for a single device and then single volume for a device.
- A volume group can have only a single extent size.
- LVM snapshots - space efficient point in time copies of volume.
```
pvcreate /dev/sdb1 // initialize a label
pvcreate --metadatacopies 2 -v /dev/sdc1 // create a second copy of LVM metadata at the end of partition.
pvdisplay /dev/sdb1 // to query the metadata information
pvremove /dev/sdb1 /dev/sdc1 // to remove a PV
pvs -v // to check the pvs
pvcreate  -vvv /dev/sdb1 /dev/sdc1
vgcreate vgtest /dev/sdb1 // create a volume group
vgdisplay vgtest // to get info about the volume group created.
vgs -v // to display information of the volume group.
vgcreate vguber --physicalextentsize 16 /dev/sdc1 // creating a volume group with increased extents.
vgextend vgtest /dev/sdc1 // extending the volume group to include the second physical bblock storage.
lvcreate -L 1G -n lvtest vgtest // creating a logical volume.
lvs -v // to see info about the logical volume.
ls -l /dev/vgtest
ls -l /dev/ | grep dm // to verify that the logical volumes are block storage
mkfs -t ext4 /dev/vgtest/lvtest // creating a file system on logical volume
mkdir /lvm
mount -t ext4 /dev/vgtest/lvtest /lvm // mount the logic volume
```


### Advanced Logical Volume Manager
- Device Mapper - Kernel based framework for advanced block storage management
    - Mapped Devices(/dev/mapper/<dev> [linear, Striped, multipathed ....]) - Mapping Layer - Target Devices (/dev/sdb /dev/sdc)
- Volume Striping - to increase IO performance
- 
```
lvextend -L2G /dev/vgtest/lvtest // to grow a logic volume
lvextend -L +1G /dev/vgtest/lvtest
e2fsck -f /dev/vgtest/lvtest // resize the file system
resize2fs /dev/vgtest/lvtest // resize the file system
// For shrinking the volume // Backup your data
umount /lvm
e2fsck -f /dev/vgtest/lvtest
resize2fs /dev/vgtest/lvtest
lvreduce -L  -2G /dev/vgtest/lvtest
e2fsck -f /dev/vgtest/lvtest
lvcreate -L 10M -s -n snaplvtest /dev/vgtest/lvtest // to create a snapshot of a volume
// reverting back to snapshot state
umount /lvm /mountsnap // unmount the origin and snapshot
lvconvert --merge /dev/vgtest/snaplvtest
 mount -t ext4 /dev/vgtest/lvtest /lvm
 cat /lvm/uberfile
// You can grow the snapshot mannually using lvextend or auto by changes in vim /etc/lvm/lvm.conf
lvcreate -L 100M --thinpool tppool vgtest // thin pool volume created
lvcreate -V 1G --thin -n tplvsmall vgtest/tppool //overprovisioning of the volume using thin pool of 100m size
lvcreate -V 100G --thin -n tpluber vgtest/tppool //over provisioning
mkdir /mnt/small /mnt/uber
mke2fs -t ext4 /dev/vgtest/tplvsmall
mount -t ext4 /dev/vgtest/tplvsmall /mnt/small
lvcreate -L 1G -n lvstripe1 -i4 vgtest // to create a Striped volume
lvcreate -L 1G -n lvstripe2 -i2 -I128  vgtest /dev/sdc1 /dev/sdd1 // to create a striped volume with specified physical disk

```


###TODO
- LVM Migration
- Backing up and Recovery
- Linux RAID
- NFS
- Samba
- ISCSI
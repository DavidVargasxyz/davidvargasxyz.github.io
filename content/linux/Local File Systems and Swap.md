
# Local File Systems and Swap

File systems 

- can be optimized, resized, mounted, and unmounted independently.
- must be connected to the directory hierarchy in order to be accessed by users and applications. This may be accomplished automatically at system boot or manually as required. File systems 
- can be mounted or unmounted using their unique identifiers, labels, or device files. \

swap space. Swapping 
- provides a mechanism to move out and in pages of idle data between physical memory and swap. 
- Swap areas act as extensions to the physical memory, and they 
- may be activated or deactivated independent of swap spaces located in other partitions and volumes.

[File Systems and File System Types](File%20Systems%20and%20File%20System%20Types.md)
[File System Management](File%20System%20Management.md)
## Monitoring File System Usage 

### df (disk free) command 
- reports usage details for mounted file systems.
- reports the numbers in KBs unless the -m or -h option is specified to view the sizes in MBs or human-readable format.

Let's run this command with the -h option on server2:
```bash
[root@server2 ~]# df -h
Filesystem             Size  Used Avail Use% Mounted on
devtmpfs               4.0M     0  4.0M   0% /dev
tmpfs                  888M     0  888M   0% /dev/shm
tmpfs                  356M  5.1M  351M   2% /run
/dev/mapper/rhel-root   17G  2.0G   15G  12% /
tmpfs                  178M     0  178M   0% /run/user/0
/dev/sda1              960M  344M  617M  36% /boot

```

Column 1:
- file system device file or type

Columns 2, 3, 4, 5, 6
- total, used, and available spaces in and the usage percentage and mount point

Useful flags

-T 
- add the file system type to the output (example: df -hT)

-x 
- exclude the specified file system type from the output (example: df -hx tmpfs)

-t 
- limit the output to a specific file system type (example: df -t xfs)

-i
- show inode information (example: df -hi)

### Calculating Disk Usage 

### du command
- reports the amount of space a file or directory occupies. 
- -m or -h option to view the output in MBs or human-readable format. In addition, you can 
- view a usage summary with the -s switch and a grand total with -c.

Run this command on the /usr/bin directory to view the usage summary:
```bash
[root@server2 ~]# du -sh /usr/bin
151M	/usr/bin

```

Add a "total" row to the output and with numbers displayed in KBs:
```bash
[root@server2 ~]# du -sc /usr/bin
154444	/usr/bin
154444	total
```

```bash
[root@server2 ~]# du -sch /usr/bin
151M	/usr/bin
151M	total
```

Try this command with different options on the /usr/sbin/lvm file and observe the results.

## Exercise 14-1: Create and Mount Ext4, VFAT, and XFS File Systems in Partitions (server2)

- create 2 x 100MB partitions on the /dev/sdb disk, 
- initialize them separately with the Ext4 and VFAT file system types, 
- define them for persistence using their UUIDs, 
- create mount points called /ext4fs1 and /vfatfs1, 
- attach them to the directorystructure
- verify their availability and usage
- you will use the disk /dev/sdc and repeat the above procedure to establish an XFS file system in it and mount it on /xfsfs1.

1\. Apply the label "msdos" to the sdb disk using the parted command:
```bash
[root@server20 ~]# sudo parted /dev/sdb mklabel msdos
Warning: The existing disk label on /dev/sdb will be destroyed and all data on this disk will be
lost. Do you want to continue?
Yes/No? y                                                                 
Information: You may need to update /etc/fstab.

```

2\. Create 2 x 100MB primary partitions on sdb with the parted command:
```bash
[root@server20 ~]# sudo parted /dev/sdb mkpart primary 1 101m
Information: You may need to update /etc/fstab.

[root@server20 ~]# sudo parted /dev/sdb mkpart primary 102 201m
Information: You may need to update /etc/fstab.

```

3\. Initialize the first partition (sdb1) with Ext4 file system type using the mkfs command:
```bash
[root@server20 ~]# sudo mkfs -t ext4 /dev/sdb1
mke2fs 1.46.5 (30-Dec-2021)
/dev/sdb1 contains a LVM2_member file system
Proceed anyway? (y,N) y
Creating filesystem with 97280 1k blocks and 24288 inodes
Filesystem UUID: 73db0582-7183-42aa-951d-2f48b7712597
Superblock backups stored on blocks: 
	8193, 24577, 40961, 57345, 73729

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (4096 blocks): done
Writing superblocks and filesystem accounting information: done 
```

4\. Initialize the second partition (sdb2) with VFAT file system type using the mkfs command:
```bash
[root@server20 ~]# sudo mkfs -t vfat /dev/sdb2
mkfs.fat 4.2 (2021-01-31)
```

5\. Initialize the whole disk (sdc) with the XFS file system type using the mkfs.xfs command. Add the -f flag to force the removal of any old partitioning or labeling information from the disk.
```bash
[root@server20 ~]# sudo mkfs.xfs /dev/sdc -f 
Filesystem should be larger than 300MB.
Log size should be at least 64MB.
Support for filesystems like this one is deprecated and they will not be supported in future releases.
meta-data=/dev/sdc               isize=512    agcount=4, agsize=16000 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=1, sparse=1, rmapbt=0
         =                       reflink=1    bigtime=1 inobtcount=1 nrext64=0
data     =                       bsize=4096   blocks=64000, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0, ftype=1
log      =internal log           bsize=4096   blocks=1368, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
```

6\. Determine the UUIDs for all three file systems using the lsblk command:
```bash
[root@server2 ~]# lsblk -f /dev/sdb /dev/sdc
NAME   FSTYPE FSVER LABEL UUID                                 FSAVAIL FSUSE% MOUNTPOINTS
sdb                                                                           
├─sdb1 ext4   1.0         0bdd22d0-db53-40bb-8cc7-36efc9184196                
└─sdb2 vfat   FAT16       FB3A-6572                                           
sdc    xfs                91884326-9686-4569-96fa-9adb02c1f6f4>)
```

7\. Open the /etc/fstab file, go to the end of the file, and append entries for the file systems for persistence using their UUIDs:
```bash
UUID=0bdd22d0-db53-40bb-8cc7-36efc9184196 /ext4fs1 ext4 defaults 0 0                
UUID=FB3A-6572 /vfatfs1 vfat defaults 0 0                                          
UUID=91884326-9686-4569-96fa-9adb02c1f6f4 /xfsfs1 xfs defaults 0 0
```

8\. Create mount points /ext4fs1, /vfatfs1, and /xfsfs1 for the three
file systems using the mkdir command:
`[root@server2 ~]# sudo mkdir /ext4fs1 /vfatfs1 /xfsfs1`

9\. Mount the new file systems using the mount command. This command will fail if there are any invalid or missing information in the file.
```bash
[root@server2 ~]# sudo mount -a
mount: (hint) your fstab has been modified, but systemd still uses
       the old version; use 'systemctl daemon-reload' to reload.
```

10\. View the mount and availability status as well as the types of all three file systems using the df command:
```bash
[root@server2 ~]# df -hT
Filesystem            Type      Size  Used Avail Use% Mounted on
devtmpfs              devtmpfs  4.0M     0  4.0M   0% /dev
tmpfs                 tmpfs     888M     0  888M   0% /dev/shm
tmpfs                 tmpfs     356M  5.1M  351M   2% /run
/dev/mapper/rhel-root xfs        17G  2.0G   15G  12% /
/dev/sda1             xfs       960M  344M  617M  36% /boot
tmpfs                 tmpfs     178M     0  178M   0% /run/user/0
/dev/sdb1             ext4       84M   14K   77M   1% /ext4fs1
/dev/sdb2             vfat       95M     0   95M   0% /vfatfs1
/dev/sdc              xfs       245M   15M  231M   6% /xfsfs1
```

## Exercise 14-2: Create and Mount Ext4 and XFS File Systems in LVM Logical Volumes (server2)


- create a volume group called vgfs comprised of a 172MB physical volume created in a partition on the /dev/sdd disk.
- The PE size for the volume group should be set at 16MB. You will 
- create two logical volumes called ext4vol and xfsvol of sizes 80MB each and initialize them with the Ext4 and XFS file system types.
- ensure that both file systems are persistently defined using their logical volume device filenames. 
- create mount points called /ext4fs2 and /xfsfs2, 
- mount the file systems, and 
- verify their availability and usage.

1\. Create a 172MB partition on the sdd disk using the parted command:
```bash
[root@server2 ~]# sudo parted /dev/sdd mkpart pri 1 172m
Information: You may need to update /etc/fstab.
```

2\. Initialize the sdd1 partition for use in LVM using the pvcreate
command:
```bash
[root@server2 ~]# sudo pvcreate /dev/sdd1
  Device /dev/sdb2 has updated name (devices file /dev/sdd2)
  Device /dev/sdb1 has no PVID (devices file brKVLFEG3AoBzhWoso0Sa1gLYHgNZ4vL)
  Physical volume "/dev/sdd1" successfully created.
```

3\. Create the volume group vgfs with a PE size of 16MB using the physical volume sdd1:
```bash
[root@server2 ~]# sudo vgcreate -s 16 vgfs /dev/sdd1
  Volume group "vgfs" successfully created

```

The PE size is not easy to alter after a volume group creation, so ensure it is defined as required at creation.

4\. Create two logical volumes ext4vol and xfsvol of size 80MB each in vgfs using the `lvcreate` command:
```bash
[root@server2 ~]# sudo lvcreate -n ext4vol -L 80 vgfs
  Logical volume "ext4vol" created.
  
[root@server2 ~]# sudo lvcreate  -n xfsvol -L 80 vgfs
  Logical volume "xfsvol" created.
```

5\. Format the ext4vol logical volume with the Ext4 file system type using the mkfs.ext4 command:
```bash
[root@server2 ~]# sudo mkfs.ext4 /dev/vgfs/ext4vol
mke2fs 1.46.5 (30-Dec-2021)
Creating filesystem with 81920 1k blocks and 20480 inodes
Filesystem UUID: 4ed1fef7-2164-485b-8035-7f627cd59419
Superblock backups stored on blocks: 
	8193, 24577, 40961, 57345, 73729

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (4096 blocks): done
Writing superblocks and filesystem accounting information: done
```

You can also use `sudo mkfs -t ext4 /dev/vgfs/ext4vol`.

6\. Format the xfsvol logical volume with the XFS file system type using the mkfs.xfs command:
```bash
[root@server2 ~]# sudo mkfs.xfs /dev/vgfs/xfsvol
Filesystem should be larger than 300MB.
Log size should be at least 64MB.
Support for filesystems like this one is deprecated and they will not be supported in future releases.
meta-data=/dev/vgfs/xfsvol       isize=512    agcount=4, agsize=5120 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=1, sparse=1, rmapbt=0
         =                       reflink=1    bigtime=1 inobtcount=1 nrext64=0
data     =                       bsize=4096   blocks=20480, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0, ftype=1
log      =internal log           bsize=4096   blocks=1368, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
```

You may also use `sudo mkfs -t xfs /dev/vgfs/xfsvol` instead.

7\. Open the /etc/fstab file, go to the end of the file, and append entries for the file systems for persistence using their device files:
```bash
/dev/vgfs/ext4vol /ext4fs2 ext4 defaults 0 0
/dev/vgfs/xfsvol /xfsfs2 xfs defaults 0 0
```

8\. Create mount points /ext4fs2 and /xfsfs2 using the mkdir command:
`[root@server2 ~]# sudo mkdir /ext4fs2 /xfsfs2`

9\. Mount the new file systems using the mount command. This command will fail if there is any invalid or missing information in the file.
```bash
[root@server2 ~]# sudo mount -a
mount: (hint) your fstab has been modified, but systemd still uses
       the old version; use 'systemctl daemon-reload' to reload.
```

10\. View the mount and availability status as well as the types of the
new LVM file systems using the lsblk and df commands:
```bash
[root@server2 ~]# lsblk /dev/sdd
NAME             MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sdd                8:48   0  250M  0 disk 
└─sdd1             8:49   0  163M  0 part 
  ├─vgfs-ext4vol 253:2    0   80M  0 lvm  /ext4fs2
  └─vgfs-xfsvol  253:3    0   80M  0 lvm  /xfsfs2
[root@server2 ~]# df -hT | grep fs2
/dev/mapper/vgfs-ext4vol ext4       70M   14K   64M   1% /ext4fs2
/dev/mapper/vgfs-xfsvol  xfs        75M  4.8M   70M   7% /xfsfs2
```

### Exercise 14-3: Resize Ext4 and XFS File Systems in LVM Logical Volumes (server 2)

- grow the size of the vgfs volume group that was created in Exercise 14-2 by adding the whole sde disk to it. 
- extend the ext4vol logical volume along with the file system it contains by 40MB using two separate commands. 
- extend the xfsvol logical volume along with the file system it contains by 40MB using a single command. 
- verify the new extensions.

1\. Initialize the sde disk and add it to the vgfs volume group:

sde had a gpt partition table with no partitions ran the following to reset it:
```bash
[root@server2 ~]# dd if=/dev/zero of=/dev/sde bs=1M count=2 conv=fsync
2+0 records in
2+0 records out
2097152 bytes (2.1 MB, 2.0 MiB) copied, 0.0102036 s, 206 MB/s
[root@server2 ~]# sudo partprobe /dev/sde
[root@server2 ~]# sudo pvcreate /dev/sde
  Physical volume "/dev/sde" successfully created.
```

```bash
[root@server2 ~]# sudo pvcreate /dev/sde
  Physical volume "/dev/sde" successfully created.
[root@server2 ~]# sudo vgextend vgfs /dev/sde
  Volume group "vgfs" successfully extended
```

2\. Confirm the new size of vgfs using the `vgs` and `vgdisplay` commands:
```bash
[root@server2 ~]# sudo vgs
  VG   #PV #LV #SN Attr   VSize   VFree  
  rhel   1   2   0 wz--n- <19.00g      0 
  vgfs   2   2   0 wz--n- 400.00m 240.00m

```

```bash
[root@server2 ~]# vgdisplay vgfs
  Devices file sys_wwid t10.ATA_VBOX_HARDDISK_VBa5e3cbf7-10921e08 PVID qeP9dCevNnTy422I8p18NxDKQ2WyDodU last seen on /dev/sdf1 not found.
  --- Volume group ---
  VG Name               vgfs
  System ID             
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  4
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               400.00 MiB
  PE Size               16.00 MiB
  Total PE              25
  Alloc PE / Size       10 / 160.00 MiB
  Free  PE / Size       15 / 240.00 MiB
  VG UUID               amDADJ-I4dH-jQUF-RFcE-58iL-jItl-5ti6LS
```


There are now two physical volumes in the volume group and the total size increased to 400MiB.

3\. Grow the logical volume ext4vol and the file system it holds by 40MB using the `lvextend` and `fsadm` command pair. Make sure to use an uppercase L to specify the size. The default unit is MiB. The plus sign (+) signifies an addition to the current size.
```bash
[root@server2 ~]# sudo lvextend -L +40 /dev/vgfs/ext4vol
  Rounding size to boundary between physical extents: 48.00 MiB.
  Size of logical volume vgfs/ext4vol changed from 80.00 MiB (5 extents) to 128.00 MiB (8 extents).
  Logical volume vgfs/ext4vol successfully resized.
  
[root@server2 ~]# sudo fsadm resize /dev/vgfs/ext4vol
resize2fs 1.46.5 (30-Dec-2021)
Filesystem at /dev/mapper/vgfs-ext4vol is mounted on /ext4fs2; on-line resizing required
old_desc_blocks = 1, new_desc_blocks = 1
The filesystem on /dev/mapper/vgfs-ext4vol is now 131072 (1k) blocks long.
```

The resize subcommand instructs the `fsadm` command to grow the file system to the full length of the specified logical volume.

4\. Grow the logical volume xfsvol and the file system (-r) it holds by (+) 40MB using the lvresize command:
```bash
[root@server2 ~]# sudo lvresize -r -L +40 /dev/vgfs/xfsvol
  Rounding size to boundary between physical extents: 48.00 MiB.
  Size of logical volume vgfs/xfsvol changed from 80.00 MiB (5 extents) to 128.00 MiB (8 extents).
  File system xfs found on vgfs/xfsvol mounted at /xfsfs2.
  Extending file system xfs to 128.00 MiB (134217728 bytes) on vgfs/xfsvol...
xfs_growfs /dev/vgfs/xfsvol
meta-data=/dev/mapper/vgfs-xfsvol isize=512    agcount=4, agsize=5120 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=1, sparse=1, rmapbt=0
         =                       reflink=1    bigtime=1 inobtcount=1 nrext64=0
data     =                       bsize=4096   blocks=20480, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0, ftype=1
log      =internal log           bsize=4096   blocks=1368, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
data blocks changed from 20480 to 32768
xfs_growfs done
  Extended file system xfs on vgfs/xfsvol.
  Logical volume vgfs/xfsvol successfully resized.
```

5\. Verify the new extensions to both logical volumes using the `lvs` command. You may also issue the `lvdisplay` or `vgdisplay` command instead.
```bash
[root@server2 ~]# sudo lvs | grep vol
  Devices file sys_wwid t10.ATA_VBOX_HARDDISK_VBa5e3cbf7-10921e08 PVID qeP9dCevNnTy422I8p18NxDKQ2WyDodU last seen on /dev/sdf1 not found.
  ext4vol vgfs -wi-ao---- 128.00m                                                    
  xfsvol  vgfs -wi-ao---- 128.00m   
```

6\. Check the new sizes and the current mount status for both file systems using the `df` and `lsblk` commands:

```bash
[root@server2 ~]# df -hT | grep -E 'ext4vol|xfsvol'
/dev/mapper/vgfs-xfsvol  xfs       123M  5.4M  118M   5% /xfsfs2
/dev/mapper/vgfs-ext4vol ext4      115M   14K  107M   1% /ext4fs2
```

```bash
[root@server2 ~]# lsblk /dev/sdd /dev/sde
NAME             MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sdd                8:48   0  250M  0 disk 
└─sdd1             8:49   0  163M  0 part 
  ├─vgfs-ext4vol 253:2    0  128M  0 lvm  /ext4fs2
  └─vgfs-xfsvol  253:3    0  128M  0 lvm  /xfsfs2
sde                8:64   0  250M  0 disk 
├─vgfs-ext4vol   253:2    0  128M  0 lvm  /ext4fs2
└─vgfs-xfsvol    253:3    0  128M  0 lvm  /xfsfs2
```

## Exercise 14-4: Create and Mount XFS File System in LVM VDO Volume 

- create an LVM VDO volume called lvvdo1 of virtual size 20GB on the 5GB sdf disk in a volume group called vgvdo1.
- initialize the volume with the XFS file system type, 
- define it for persistence using its device files, 
- create a mount point called /xfsvdo1, attach it to the directory structure, 
- verify its availability and usage.\

1\. Initialize the sdf disk using the `pvcreate` command:
```bash
[root@server2 ~]# sudo pvcreate /dev/sdf
  WARNING: adding device /dev/sdf with idname t10.ATA_VBOX_HARDDISK_VBa5e3cbf7-10921e08 which is already used for missing device.
  Physical volume "/dev/sdf" successfully created.
```

2\. Create vgvdo1 volume group using the `vgcreate` command:
```bash
[root@server2 ~]# sudo vgcreate vgvdo1 /dev/sdf
  WARNING: adding device /dev/sdf with idname t10.ATA_VBOX_HARDDISK_VBa5e3cbf7-10921e08 which is already used for missing device.
  Volume group "vgvdo1" successfully created
```

3\. Display basic information about the volume group:
```bash
root@server2 ~]# sudo vgdisplay vgvdo1
  Devices file sys_wwid t10.ATA_VBOX_HARDDISK_VBa5e3cbf7-10921e08 PVID qeP9dCevNnTy422I8p18NxDKQ2WyDodU last seen on /dev/sdf1 not found.
  --- Volume group ---
  VG Name               vgvdo1
  System ID             
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <5.00 GiB
  PE Size               4.00 MiB
  Total PE              1279
  Alloc PE / Size       0 / 0   
  Free  PE / Size       1279 / <5.00 GiB
  VG UUID               b9u8Ng-m3BF-Jz2b-sBu8-gEG1-bBGQ-sBgrt0

```

4\. Create a VDO volume called lvvdo1 using the `lvcreate` command. Use the `-l` option to specify the number of logical extents (1279) to be allocated and the `-V` option for the amount of virtual space (20GB).
```bash
[root@server2 ~]# sudo lvcreate -n lvvdo -l 1279 -V 20G --type vdo vgvdo1
WARNING: vdo signature detected on /dev/vgvdo1/vpool0 at offset 0. Wipe it? [y/n]: y
  Wiping vdo signature on /dev/vgvdo1/vpool0.
    The VDO volume can address 2 GB in 1 data slab.
    It can grow to address at most 16 TB of physical storage in 8192 slabs.
    If a larger maximum size might be needed, use bigger slabs.
  Logical volume "lvvdo" created.
```

5\. Display detailed information about the volume group including the logical volume and the physical volume:
```bash
[root@server2 ~]# sudo vgdisplay -v vgvdo1
  Devices file sys_wwid t10.ATA_VBOX_HARDDISK_VBa5e3cbf7-10921e08 PVID qeP9dCevNnTy422I8p18NxDKQ2WyDodU last seen on /dev/sdf1 not found.
  --- Volume group ---
  VG Name               vgvdo1
  System ID             
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  3
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               0
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <5.00 GiB
  PE Size               4.00 MiB
  Total PE              1279
  Alloc PE / Size       1279 / <5.00 GiB
  Free  PE / Size       0 / 0   
  VG UUID               b9u8Ng-m3BF-Jz2b-sBu8-gEG1-bBGQ-sBgrt0
   
  --- Logical volume ---
  LV Path                /dev/vgvdo1/vpool0
  LV Name                vpool0
  VG Name                vgvdo1
  LV UUID                nTPKtv-3yTW-J7Cy-HVP1-Aujs-cXZ6-gdS2fI
  LV Write Access        read/write
  LV Creation host, time server2, 2024-07-01 12:57:56 -0700
  LV VDO Pool data       vpool0_vdata
  LV VDO Pool usage      60.00%
  LV VDO Pool saving     100.00%
  LV VDO Operating mode  normal
  LV VDO Index state     online
  LV VDO Compression st  online
  LV VDO Used size       <3.00 GiB
  LV Status              NOT available
  LV Size                <5.00 GiB
  Current LE             1279
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
   
  --- Logical volume ---
  LV Path                /dev/vgvdo1/lvvdo
  LV Name                lvvdo
  VG Name                vgvdo1
  LV UUID                Z09BdK-ETJk-Gi53-m8Cg-mnTd-RYug-Z9nV0L
  LV Write Access        read/write
  LV Creation host, time server2, 2024-07-01 12:58:02 -0700
  LV VDO Pool name       vpool0
  LV Status              available
  # open                 0
  LV Size                20.00 GiB
  Current LE             5120
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:6
   
  --- Physical volumes ---
  PV Name               /dev/sdf     
  PV UUID               WKc956-Xp66-L8v9-VA6S-KWM5-5e3X-kx1v0V
  PV Status             allocatable
  Total PE / Free PE    1279 / 0
```

6\. Display the new VDO volume creation using the lsblk command:
```bash
[root@server2 ~]# sudo lsblk /dev/sdf
NAME                    MAJ:MIN RM SIZE RO TYPE MOUNTPOINTS
sdf                       8:80   0   5G  0 disk 
└─vgvdo1-vpool0_vdata   253:4    0   5G  0 lvm  
  └─vgvdo1-vpool0-vpool 253:5    0  20G  0 lvm  
    └─vgvdo1-lvvdo      253:6    0  20G  0 lvm  
```

The output shows the virtual volume size (20GB) and the underlying disk size (5GB).

7\. Initialize the VDO volume with the XFS file system type using the `mkfs.xfs` command. The VDO volume device file is
/dev/mapper/vgvdo1-lvvdo1 as indicated in the above output. Add the `-f` flag to force the removal of any old partitioning or labeling information from the disk.
```bash
[root@server2 mapper]# sudo mkfs.xfs /dev/mapper/vgvdo1-lvvdo
meta-data=/dev/mapper/vgvdo1-lvvdo isize=512    agcount=4, agsize=1310720 blks
         =                       sectsz=4096  attr=2, projid32bit=1
         =                       crc=1        finobt=1, sparse=1, rmapbt=0
         =                       reflink=1    bigtime=1 inobtcount=1 nrext64=0
data     =                       bsize=4096   blocks=5242880, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0, ftype=1
log      =internal log           bsize=4096   blocks=16384, version=2
         =                       sectsz=4096  sunit=1 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
Discarding blocks...Done.
```

(lab said vgvdo1-lvvdo1 but it didn't exist for me.)

8\. Open the /etc/fstab file, go to the end of the file, and append the following entry for the file system for persistent mounts using its device file:
```bash
/dev/mapper/vgvdo1-lvvdo /xfsvdo1 xfs defaults 0 0 
```

9\. Create the mount point /xfsvdo1 using the mkdir command:
```bash
[root@server2 mapper]# sudo mkdir /xfsvdo1
```

10\. Mount the new file system using the mount command. This command will fail if there are any invalid or missing information in the file.
```bash
[root@server2 mapper]# sudo mount -a
mount: (hint) your fstab has been modified, but systemd still uses
       the old version; use 'systemctl daemon-reload' to reload.
```

The mount command with the `-a` flag is a validation test for the fstab file. It should always be executed after updating this file and before rebooting the server to avoid landing the system in an unbootable state.

11\. View the mount and availability status as well as the type of the VDO file system using the `lsblk` and `df` commands:
```bash
[root@server2 mapper]# lsblk /dev/sdf
NAME                    MAJ:MIN RM SIZE RO TYPE MOUNTPOINTS
sdf                       8:80   0   5G  0 disk 
└─vgvdo1-vpool0_vdata   253:4    0   5G  0 lvm  
  └─vgvdo1-vpool0-vpool 253:5    0  20G  0 lvm  
    └─vgvdo1-lvvdo      253:6    0  20G  0 lvm  /xfsvdo1

[root@server2 mapper]# df -hT /xfsvdo1
Filesystem               Type  Size  Used Avail Use% Mounted on
/dev/mapper/vgvdo1-lvvdo xfs    20G  175M   20G   1% /xfsvdo1
```

# Stuff
[Swap and its Management](Swap%20and%20its%20Management.md)
[File Systems and Swap Review Questions](File%20Systems%20and%20Swap%20Review%20Questions.md)
[Remove a filesystem from a partition](Remove%20a%20filesystem%20from%20a%20partition.md)
[Local Filesystems and Swap DIY Labs](Local%20Filesystems%20and%20Swap%20DIY%20Labs.md)
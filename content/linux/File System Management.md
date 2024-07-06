## File System Management 

### File System Administration Commands 

- Some are limited to their operations on the Extended, XFS, or VFAT file system type, while  
- others are general and applicable to all file system types. 


#### Extended File System Management Commands            

`e2label`                             
  - Modifies the label of a file system

`tune2fs`                            
  - Tunes or displays file system attributes

#### XFS Management Commands                             

`xfs_admin  `                         
 - Tunes file system attributes

`xfs_growfs `                         
  - Extends the size of a file system

`xfs_info  `                          
- Exhibits information about a file system

#### General File System Commands        

`blkid  `                             
  - Displays block device attributes including their UUIDs and labels

`df `                                 
  - Reports file system utilization

`du   `                               
- Calculates disk usage of directories and file systems

`fsadm   `                            
- Resizes a file system. This command is automatically invoked when the lvresize command is run with the -r switch.

`lsblk  `                             
- Lists block devices and file systems and their attributes including their UUIDs and labels

`mkfs  `                              
- Creates a file system. Use the -t option and specify ext3, ext4, vfat, or xfs file system type.

`mount    `                           
- Mounts a file system for user access. Displays currently mounted file systems.

`umount   `                           
- Unmounts a file system

### Mounting and Unmounting File Systems 

- file system must be connected to the directory structure at a desired attachment point, (mount point) 
- A mount point in essence is any empty directory that is created and used for this purpose.

use the mount command to view information about mounted file systems. The following shows the
XFS file systems only:
```bash
[root@server2 ~]# mount -t xfs
/dev/mapper/rhel-root on / type xfs (rw,relatime,seclabel,attr2,inode64,logbufs=8,logbsize=32k,noquota)
/dev/sda1 on /boot type xfs (rw,relatime,seclabel,attr2,inode64,logbufs=8,logbsize=32k,noquota)
```

#### Mount command
-t option 
- type. 

- used for mounting a file system to a mount point
- performed with the root user privileges.
- requires the absolute pathnames of the file system block device and the mount point name. 
- accepts the UUID or label of the file system in lieu of the block device name. 
- mount all or a specific type of file system. 
- Upon successful mount, the kernel places an entry for the file system in the /proc/self/mounts file.
- A mount point should be empty when an attempt is made to mount a file system on it, otherwise the content of the mount point will hide.
- the mount point must not be in use or the mount attempt will fail.

auto (noauto)                       
- Mounts (does not mount) the file system when the -a option is specified

defaults                            
- Mounts a file system with all the default values (async, auto, rw, etc.)

`_netdev`                         
- Used for a file system that requires network connectivity in place before it can be mounted. NFS is an example.

remount                             
- Remounts an already mounted file system to enable or disable an option

ro (rw)                             
- Mounts a file system read-only read/write)

#### umount Command
- used to detach a file system from the directory hierarchy and make it inaccessible to users and applications. 
- expects the absolute pathname to the block device containing the file system or its mount point name in order to detach it. 
- umount to unmount all or a specific type of file system. 
- kernel removes the corresponding file system entry from the /proc/self/mounts file after it has been successfully disconnected.
### Determining the UUID of a File System 

- Every Extended and XFS file system has a 128-bit (32 hexadecimal characters) UUID (Universally Unique IDentifier) assigned to it at the time of its creation. In contrast, 
- UUIDs assigned to vfat file systems are 32-bit (8 hexadecimal characters) in length. 
- Assigning a UUID makes the file system unique among many other file systems that potentially exist on the system. 
- persistent across system reboots. 
- used by default in RHEL 9 in the /etc/fstab file for any file system that is created by the system in a standard partition.

- RHEL attempts to mount all file systems listed in the /etc/fstab file at reboots. 
- Each file system has an associated device file and UUID, but may or may not have a corresponding label. 
- The system checks for the presence of each file system's device file, UUID, or label, and then attempts to mount it.

Determine the UUID of /boot
```bash
[root@server2 ~]# lsblk | grep boot
├─sda1          8:1    0    1G  0 part /boot
```

```bash
[root@server2 ~]# sudo xfs_admin -u /dev/sda1
UUID = 630568e1-608f-4603-9b97-e27f82c7d4b4

[root@server2 ~]# sudo blkid /dev/sda1
/dev/sda1: UUID="630568e1-608f-4603-9b97-e27f82c7d4b4" TYPE="xfs" PARTUUID="7dcb43e4-01"

[root@server2 ~]# sudo lsblk -f /dev/sda1
NAME FSTYPE FSVER LABEL UUID                                 FSAVAIL FSUSE% MOUNTPOINTS
sda1 xfs                630568e1-608f-4603-9b97-e27f82c7d4b4  616.1M    36% /boot
```

For extended file systems, you can use the `tune2fs`, `blkid`, or `lsblk` commands to determine the UUID.

A UUID is also assigned to a file system that is created in a VDO or LVM volume; however, it need not be used in the fstab file, as the device
files associated with the logical volumes are always unique and persistent.

### Labeling a File System 

- A unique label may be used instead of a UUID to keep the file system association with its device file exclusive and persistent across system reboots. 
- A label is limited to a maximum of 12 characters on the XFS file system 
- 16 characters on the Extended file system. 
- By default, no labels are assigned to a file system at the time of its creation.

The /boot file system is located in the /dev/sda1 partition and its type is XFS. You can use the xfs_admin or the lsblk command as follows to
determine its label:
```bash
[root@server2 ~]# sudo xfs_admin -l /dev/sda1
label = ""

[root@server2 ~]# sudo lsblk -f /dev/sda1
NAME FSTYPE FSVER LABEL UUID                                 FSAVAIL FSUSE% MOUNTPOINTS
sda1 xfs                630568e1-608f-4603-9b97-e27f82c7d4b4  616.1M    36% /boot
```

- not needed on a file system if you intend to use its UUID or if it is created in a logical volume
- you can still apply one using the `xfs_admin` command with the `-L` option. 
- Labeling an XFS file system requires that the target file system be unmounted.

unmount /boot, set the label "bootfs" on its device file, and remount it:
```bash
[root@server2 ~]# sudo umount /boot
[root@server2 ~]# sudo xfs_admin -L bootfs /dev/sda1
writing all SBs
new label = "bootfs"

```

confirm the new label by executing `sudo xfs_admin -l /dev/sda1` or `sudo lsblk -f /dev/sda1`.

For extended file systems, you can use the `e2label` command to apply a label and the `tune2fs`, `blkid`, and `lsblk` commands to view and verify.

Now you can replace the `UUID=\"22d05484-6ae1-4ef8-a37d-abab674a5e35`\" for /boot in the fstab file with `LABEL=bootfs`, and unmount and remount /boot as demonstrated above for confirmation.
```bash
[root@server2 ~]# mount /boot
mount: (hint) your fstab has been modified, but systemd still uses
       the old version; use 'systemctl daemon-reload' to reload.
```

A label may also be applied to a file system created in a logical volume; however, it is not recommended for use in the fstab file, as the
device files for logical volumes are always unique and remain persistent across system reboots.

### Automatically Mounting a File System at Reboots 

#### /etc/fstab
- File systems defined in the /etc/fstab file are mounted automatically at reboots. 
- must contain proper and complete information for each listed file system. 
- An incomplete or inaccurate entry might leave the system in an undesirable or unbootable state. 
- only need to specify one of the four attributes---block device name, UUID, label, or mount point---of the file system that you wish to mount manually with the mount command. 
- The mount command obtains the rest of the information from this file. 
- only need to specify one of these attributes with the umount command to detach it from the directory hierarchy.
- contains entries for file systems that are created at the time of installation. 

```bash
[root@server2 ~]# cat /etc/fstab

#
# /etc/fstab
# Created by anaconda on Sun Feb 25 12:11:47 2024
#
# Accessible filesystems, by reference, are maintained under '/dev/disk/'.
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info.
#
# After editing this file, run 'systemctl daemon-reload' to update systemd
# units generated from this file.
#
/dev/mapper/rhel-root   /                       xfs     defaults        0 0
LABEL=bootfs /boot                   xfs     defaults        0 0
/dev/mapper/rhel-swap   none                    swap    defaults        0 0
```

EXAM TIP: Any missing or invalid entry in this file may render the system unbootable. You will have to boot the system in emergency mode to
fix this file. Ensure that you understand each field in the file for both file system and swap entries.

The format of this file is such that each row is broken out into six columns to identify the required attributes for each file system to be
successfully mounted. Here is what the columns contain:

Column 1: 
- physical or virtual device path where the file system is resident, or its associated UUID or label. 
- can be entries for network file systems here as well.

Column 2: 
- Identifies the mount point for the file system. 
- swap partitions, use either "none" or "swap".

Column 3: 
- type of file system such as Ext3, Ext4, XFS, VFAT, or ISO9660. 
- For swap, the type "swap" is used.  
- may use "auto" instead to leave it up to the mount command to determine the type of the file system.

Column 4: 
- Identifies one or more comma-separated options to be used when mounting the file system. 
- consult the manual pages of the mount command or the fstab file for additional options and details.

Column 5: 
- used by the dump utility to ascertain the file systems that need to be dumped. 
- value of 0 (or the absence of this column) disables this check. 
- This field is applicable only on Extended file systems; 
- XFS does not use it.

Column 6: 
- sequence number in which to run the e2fsck (file system check and repair utility for Extended file system types) utility on the file system at system boot. 
- By default, 0 is used for memory-based, remote, and removable file systems, 1 for /, and 2 for /boot and other physical file systems. 0 can also be used for /, /boot, and other physical file systems you don't want to be checked or repaired. 
- applicable only on Extended file systems; 
- XFS does not use it.

- 0 in columns 5 and 6 for XFS, virtual, remote, and removable file system types has no meaning. You do not need to add them for these file system types.

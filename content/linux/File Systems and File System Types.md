## File Systems and File System Types 

- Each file system is created in a discrete partition, VDO volume, or logical volume. 
- A typical production RHEL system usually has numerous file systems. 
- During OS installation, only two file systems---/ and /boot---are created in the default disk layout, but you can design a custom disk layout and construct separate containers to store dissimilar
information. 
- Typical additional file systems that may be created during an installation are /home, /opt, /tmp, /usr, and /var. 
- The two mandatory file systems---/ and /boot---are required for installation and booting.

Storing disparate data in distinct file systems versus storing all data in a single file system offers the following advantages:

- Make any file system accessible (mount) or inaccessible (unmount) to users independent of other file systems. This hides or reveals information contained in that file system.
- Perform file system repair activities on individual file systems
- Keep dissimilar data in separate file systems
- Optimize or tune each file system independently
- Grow or shrink a file system independent of other file systems

3 types of file system: disk-based, network-based, and memory-based.

**Disk-based**
- typically created on physical drives using SATA, USB, Fibre Channel, and other technologies. 
 
**Network-based** 
- essentially disk-based file systems shared over the network for remote access. 
- 
**Memory-based** 
- virtual
- created at system startup and destroyed when the system goes down.

Disk-based and network-based file systems store information persistently, while any data saved in virtual file systems does not
survive across system reboots.


  Ext3
    -  Disk                   
    -  The third generation of the extended filesystem. It supports metadata journaling for faster recovery, offer superior reliability, allows the creation of up to 32,000 subdirectories, and supports larger file systems and bigger files than its predecessor.

  Ext4
    - Disk
    - successor to Ext3. It 
    - supports all features of Ext3 in addition to:
	    - a larger file system size, 
	    - bigger file size, an 
	    - unlimited number of subdirectories, 
	    - metadata and quota journaling, and 
	    - extended user attributes.

  XFS                     
    - Disk 
    - XFS is a highly scalable and high-performing 64-bit file system. It
    - supports:
	    - metadata journaling for faster crash recovery, and 
	    - online defragmentation, expansion, quota journaling, and extended user attributes. XFS is the 
	- default file system type in RHEL 9.

  VFAT                    
  - Disk                 
  - used for post-Windows 95 file system formats on hard disks, USB drives, and floppy disks.

  ISO9660                 
  - Disk                    This is 
  - used for optical file systems such as CD and DVD.

  NFS - (Network File System.)          
  - Network                
  - shared directory or file system for remote access by other Linux systems.

  AutoFS  (Auto File System)               
  - Network 
  - NFS file system set to mount and unmount automatically on remote client systems.

### Extended File Systems

- first generation is obsolete and is no longer supported
- second, third, and fourth generations are currently available and supported. 
- fourth generation is the latest in the series and is superior in features and enhancements to its predecessors.
- structure is built on a partition or logical volume at the time of file system creation. This structure is divided into two sets: 
	- **first set** holds the file system's **metadata** and it is very tiny. 
		- includes superblock
			- keeps vital file system structural information:
				- type
				- size
				- status of the file system
				- number of data blocks it contains
				- automatically replicated and maintained at various known locations throughout the file system. 
				- primary superblock
					- superblock at the beginning of the file system 
				- backup superblocks. 
					- I used to supplant the corrupted or lost primary superblock to bring the file system back to its normal state.
					- Copy of the primary
		- contains the inode table
			- maintains a list of index node (inode) numbers. 
			- Each file is assigned an **inode number** at the time of its creation, and the inode number
				- holds the file's attributes such as:
					- type, 
					- permissions, 
					- ownership, 
					- owning group, 
					- size
					- last access/modification time
					- holds and keeps track of the pointers to the actual data blocks where the file contents are located.
	- **second set** stores the actual data, and it occupies almost the entire partition or the logical volume (VDO and LVM) space.\

**journaling** 
- Supported by Ext3 and Ext4
- provides the ability to recover swiftly after a system crash.
- keep track of recent changes in their metadata in a journal (or log). 
- Each metadata update is written in its entirety to the journal after completion. 
- The system peruses the journal of each extended file system following the reboot after a crash to determine if there are any errors, 
- Lets the system recover the file system rapidly using the latest metadata information stored in its journal.

- Ext3 that supports file systems up to 16TiB and files up to 2TiB, 
- Ext4 supports very large file systems up to 1EiB (ExbiByte) and files up to 16TiB (TebiByte).
	- uses a series of contiguous physical blocks on the hard disk called extents, resulting in improved read and write performance with reduced fragmentation. 
	- supports extended user attributes, metadata and quota journaling, etc.

### XFS File System

- high-performing 64-bit extent-based journaling file system type. XFS 
- allows the creation of file systems and files up to 8EiB (ExbiByte). It 
- does not run file system checks at system boot; rather, it 
- relies on you to use the xfs_repair utility to manually fix any issues. XFS 
- sets the extended user attributes and certain mount options by default on new file systems. It 
- enables defragmentation on mounted and active file systems to keep as much data in contiguous blocks as possible for faster access. 
- inability to shrink.
- uses journaling for metadata operations, guaranteeing the consistency of the file system against abnormal or forced unmounting. The
- journal information is read and any pending metadata transactions are replayed when the XFS file system is remounted.
- speedy input/output performance. It 
- can be snapshot in a mounted, active state.

### VFAT File System 

- extension to the legacy FAT file system (FAT16)
- supports 255 characters in filenames including spaces and periods; however, it still  
- does not differentiate between lowercase and uppercase letters. 
- primarily used on removable media, such as floppy and USB flash drives, for exchanging data between Linux and Windows.

### ISO9660 File System

- used for removable optical disc media such as CD/DVD drives

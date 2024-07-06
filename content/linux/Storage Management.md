# Storage Management
- Data is stored on disks that are logically divided into partitions. 
- A partition can exist on a portion of a disk, on an entire disk, or it may span multiple disks. 
- Each partition is accessed and managed independent of other partitions and may contain a file system or swap space.
- Partitioning information is stored at special disk locations that the system references at boot time. 
- Partitions created with a combination of most tools can coexist on a single disk.
- Thin provisioning is a powerful feature that guarantees an efficient use of storage space by allocating only what is needed and by storing data at adjacent locations. 
- Many storage management solutions incorporate thin provisioning technology in their core configuration.
- Virtual Data Optimizer capitalizes on thin provisioning, de-duplication, and compression technologies to conserve storage space, improve data throughput, and save money.
- The Logical Volume Manager solution sets up an abstraction layer between the operating system and the storage hardware. It utilizes virtual objects for storage pooling and allocation, and offers a slew of commands to carry out management operations.

# Storage topics

[Adding Storage for Practice](Adding%20Storage%20for%20Practice.md)
[Partition Information (MBR and GPT)](Partition%20Information%20(MBR%20and%20GPT).md)
[Disk Partitions](Disk%20Partitions.md)
[Thin Provisioning](Thin%20Provisioning.md)
[Logical Volume Manager (LVM)](Logical%20Volume%20Manager%20(LVM).md)
[Storage Optimization with Virtual Data Optimizer (VDO)](Storage%20Optimization%20with%20Virtual%20Data%20Optimizer%20(VDO).md)
[Storage Review Questions](Storage%20Review%20Questions.md)
[Storage DYI Labs](Storage%20DYI%20Labs.md)
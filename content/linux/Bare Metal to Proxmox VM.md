Create a vm in proxmox and copy the physical drive to the vm drive.

1. Create New VM in proxmox
2. In the OS tab, select "Do not use any media"
![](Pasted%20image%2020240806075409.png)

Verify Kernel version
```bash
[root@server10 conf.d]# hostnamectl
   Static hostname: server10.example.com          
		   Icon name: computer-server
           Chassis: server
        Machine ID: 22baa6004a0f4bb59217ab20b4d981c7
           Boot ID: 35a631cc684348a4872db20ddb169a8a
  Operating System: AlmaLinux 8.10 (Cerulean Leopard)
       CPE OS Name: cpe:/o:almalinux:almalinux:8::baseos
            Kernel: Linux 4.18.0-553.8.1.el8_10.x86_64
      Architecture: x86-64
```

Check the boot partition for BIOS or UEFI
```bash
[root@dall conf.d]# lsblk
NAME             MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda                8:0    0 931.5G  0 disk 
├─sda1             8:1    0   600M  0 part /boot/efi <---- 
├─sda2             8:2    0     1G  0 part /boot
└─sda3             8:3    0 929.9G  0 part 
  ├─cl_dall-root 253:0    0   150G  0 lvm  /
  ├─cl_dall-swap 253:1    0  15.6G  0 lvm  [SWAP]
  ├─cl_dall-home 253:2    0 186.3G  0 lvm  /home
  └─cl_dall-var  253:3    0   578G  0 lvm  /var

```

Select the appropriate BIOS in the System tab:
![](Pasted%20image%2020240806080050.png)
Check your disk size
```bash
[root@dall conf.d]# lsblk
NAME             MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda                8:0    0 931.5G  0 disk 
```

Make the disk a little bit bigger than the physical disk
![](Pasted%20image%2020240806080538.png)

Select CPU and Memory based on requirements.

Select virtIO for network card (Linux has drivers for this pre installed.)

Confirm

### Copy data to hard drive
- Create an NFS share on the proxmox server

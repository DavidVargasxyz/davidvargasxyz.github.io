## Basic Package Management Labs

### Lab: Mount RHEL 9 ISO Persistently
1. Go to the VirtualBox VM Manager and make sure that the RHEL 8 image is attached to RHEL9-VM1 as depicted below:
![](Pasted%20image%2020240713164339.png)

2. Open the /etc/fstab file in the vim editor (or another editor of your choice) and add the following line entry at the end of the file to mount the DVD image (/dev/sr0) in read-only (ro) mode on the /mnt directory.

	```
	/dev/sr0 /mnt iso9660 ro 0 0
	```

Note: sr0 represents the first instance of the optical device and iso9660 is the standard format for optical file systems.

3. Mount the file system as per the configuration defined in the /etc/fstab file using the mount command with the -a (all) option:

	```
	sudo mount -a
	```

4. Verify the mount using the df command:

	```
	df -h | grep mnt
	```

Note: The image and the packages therein can now be accessed via the /mnt directory just like any other local directory on the system.

5. List the two directories—/mnt/BaseOS/Packages and /mnt/AppStream/Packages—that contain all the software packages (directory names are case sensitive):

	```
	ls -l /mnt/BaseOS/Packages | more
	```

### Lab: Query Packages (RPM)
1. query all installed packages:
	`rpm -qa`

2. query whether the perl package is installed:
	`rpm -q perl`

3. list all files in a package:
	`rpm -ql iproute`

4. list only the documentation files in a package:
	`rpm -qd audit`

5. list only the configuration files in a package:
	`rpm -qc cups`

6. identify which package owns the specified file:
	`rpm -qf /etc/passwd`

7. display information about an installed package including version, release, installation status, installation date, size, signatures, description, and so on:
	`rpm -qi setup`

8. list all file and package dependencies for a given package:
	`rpm -qR chrony`

9. query an installable package for metadata information (version, release, architecture, description, size, signatures, etc.):
	`rpm -qip /mnt/BaseOS/Packages/zsh-5.5.1-6.el8.x86_64.rpm`

10. determine what packages require the specified package in order to operate properly:
	`rpm -q --whatrequires lvm2`


### Lab: Installing a Package (RPM)
1. Install zsh-5.5.1-6.el8.x86_64.rpm
`sudo rpm -ivh /mnt/BaseOS/Packages/zsh-5.5.1-6.el8.x86_64.rpm`

### Lab: Upgrading a Package (RPM)
1. Upgrade sushi with the -U option:
`sudo rpm -Uvh /mnt/AppStream/Packages/sushi-3.28.3-1.el8.x86_64.rpm`
### Lab: Freshening a Package
1. Freshen the sushi package:
`sudo rpm -Fvh /mnt/AppStream/Packages/sushi-3.28.3-1.el8.x86_64.rpm`


### Lab: Overwriting a Package
1. Overwrite zsh-5.5.1-6.el8.x86_64
`sudo rpm -ivh --replacepkgs /mnt/BaseOS/Packages/zsh-5.5.1-6.el8.x86_64`

### Lab: Removing a Package
1. Remove sushi
`sudo rpm sushi -ve`
``

### Lab: Extracting Files from an Installable Package
1. You have lost /etc/crony.conf. Determine what package this file comes from:
`rpm -qf /etc/chrony.conf`

2. Extract all files from the crony package to /tmp and create the directory structure:
```
cd /tmp
sudo rpm2cpio /mnt/BaseOS/Packages/chrony-3.3-3.el8.x86_64.rpm | cpio -imd
1066 blocks
```

3. Use find to locate the crony.conf file:
`sudo find . -name chrony.conf`

4. Copy the file to /etc:


### Lab: Validating Package Integrity and Credibility
1. Check the integrity of zsh-5.5.1-6.el8.x86_64.rpm located in /mnt/BaseOS/Packages:
`rpm -K /mnt/BaseOS/Packages/zsh-5.5.1-6.el8.x86_64.rpm --nosignature`
2. Import the GPG key from the proper file and verify the signature for the zsh-5.5.1-6.el8.x86_64.rpm package. 
```
sudo rpmkeys --import /etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release
sudo rpmkeys -K /mnt/BaseOS/Packages/zsh-5.5.1-6.el8.x86_64.rpm
```

### Lab: Viewing GPG Keys
1. List the imported key: 
`rpm -q gpg-pubkey`
2. View details for the first key:
`rpm -qi gpg-pubkey-fd431d51-4ae0493b`

### Lab: Verifying Package Attributes
1. Run a check on the at program:
`sudo rpm -V at`
2. Change permissions of one of the files and run the check again:
```
ls -l /etc/sysconfig/atd
sudo chmod -v 770 /etc/sysconfig/atd
sudo rpm -V at
```

3. Run the check directly on the file:
`sudo rpm -Vf /etc/sysconfig/atd`

4. Reset the value and check the file again:
```
sudo chmod -v 644 /etc/sysconfig/atd
sudo rpm -V at
```



### Lab: Perform Package Management Using rpm
1. Run the ls command on the /mnt/AppStream/Packages directory to confirm that the dcraw package is available:
`ls -l /mnt/AppStream/Packages/dcraw*`

2. Run the rpm command and verify the integrity and credibility of the package:
`rpmkeys -K /mnt/AppStream/Packages/dcraw-9.27.0-9.el8.x86_64.rpm`

3. Install the Package:
`sudo rpm -ivh /mnt/AppStream/Packaghes/dcraw-9.27.0-9.el8x86_64.rpm`

4. Show basic information about the package:
`rpm -qi dcraw`

5. Show all the files the package contains:
`rpm -ql dcraw`

6. List the documentation files the package has:
`rpm -qd dcraw`

7. Verify the attributes of each file in the package. Use verbose mode.
`rpm -Vv dcraw`

8. Remove the package:
`sudo rpm -ve dcraw`


```
This process worked at creating an image for me, but resulted in a highly unstable system that crashes. Will revisit this at another time. 
```

You will need to download a Fedora Live iso here: https://fedoraproject.org/en/workstation/download
Then install `livecd-iso-to-mediums` to provide `livecd-iso-to-disk`
`dnf -y install livecd-iso-to-mediums`

Create a file called `livecd-iso-to-disk.f38.patch.txt` and add the following text to it:
```bash
--- /usr/bin/livecd-iso-to-disk 2022-06-15 04:49:24.000000000 -0700
+++ livecd-iso-to-disk.patched  2023-04-27 09:11:13.665060837 -0700
@@ -1058,7 +1058,7 @@ createFSLayout() {
         udevadm settle -E $p2
         mkfs.fat -v -n ESP $p2
         umount $p2 &> /dev/null || :
-        losetup -d $l2
+        losetup -d $l2 &> /dev/null || :
         fsck.fat -avVw $p2
         echo
     fi
@@ -2021,7 +2021,16 @@ if [[ -d $SRCMNT/isolinux/ ]]; then
 elif [[ -d $SRCMNT/syslinux/ ]]; then
     [[ -d $SRCMNT/$srcdir/syslinux ]] && CONFIG_SRC="$srcdir"/
     CONFIG_SRC="$SRCMNT/${CONFIG_SRC}syslinux"
+elif [[ -d $SRCMNT/images/pxeboot ]]; then
+    CONFIG_SRC="$SRCMNT/images/pxeboot"
 fi
+
+if [[ ! -d ${CONFIG_SRC} ]]; then
+  printf '\n        ATTENTION:
+  A bootable kernel and initial ram disk could not be found.  Exiting...\n'
+  exitclean
+fi
+
 i=${CONFIG_SRC}/initrd*.img
 cd /tmp
 rm -rf usr
@@ -2263,11 +2272,15 @@ if [[ -n ${format[1]} && -z $skipcopy ]]
                 ((free < 30*10**9)) && ((p2s+=1<<27)) ||
                     ((p2s+=free/(15*10**9)<<27))
             fi
-            # Guarantee that there is at least 1 MiB of extra space for a gap.
-            ((oio-p2s%oio < 1<<20)) && ((p2s+=1<<20))
-            # Set partition size to whole 4-MiB or OPT-IO units.
-            ((p2s=(p2s/oio+1)*oio))
+        else
+            # Force a 100 MB ESP partition
+            p2s=100*1024*1024
         fi
+        # Guarantee that there is at least 1 MiB of extra space for a gap.
+        ((oio-p2s%oio < 1<<20)) && ((p2s+=1<<20))
+        # Set partition size to whole 4-MiB or OPT-IO units.
+        ((p2s=(p2s/oio+1)*oio))
+
         if [[ -z $nomac ]]; then
             l3=$SRCMNT/images/macboot.img
             ! [[ -f $l3 ]] && l3=$SRCMNT/isolinux/macboot.img
@@ -2742,6 +2755,16 @@ if ! [[ -f $BOOTCONFIG ]]; then
         [[ -f $f ]] && mv $f $BOOTCONFIG && break
     done
 fi
+if [[ ! -f $BOOTCONFIG ]]; then
+    # Dummy-up a boot configuration file when missing for EFI boot
+    if [[ -n $efi ]]; then
+       echo "#### Unused dummy boot loader configuration file ####" > $BOOTCONFIG 
+    else
+       echo "ERROR: Unable to find a non-UEFI boot configuration file."
+       exitclean
+    fi
+fi
+
 TITLE=$(sed -n -r '/^\s*label\s+linux/I {n
                    s/^\s*menu\s+label\s+\^\S+\s+(.*)/\1/Ip;q}
                   ' $BOOTCONFIG)                               
```

Or you can grab the file from this thread: 
https://github.com/livecd-tools/livecd-tools/issues/262

To apply the patch:
```bash
cd /usr/bin && patch --input=~/Documents/livecd-iso-to-disk.f38.patch.txt livecd-iso-to-disk
```

Then find the device name using lsblk:
```bash
dthomas@lgfedora:~$ lsblk
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sdc           8:32   1 466.3G  0 disk 
└─sdc1        8:33   1 466.2G  0 part 
```

Make sure you are using the right device here. The Disks Utility in Fedora can provide the same information. 
![](Pasted%20image%2020240808100730%201.png)

You can find more options for the `live-iso-to-disk` command here: https://docs.fedoraproject.org/en-US/quick-docs/creating-and-using-a-live-installation-image/#_livecd_iso_to_disk_problems

But this command will create a persistent usb and all of the directories using a 500gb usb drive. You will need to adjust the values based on your drive size. 
```bash
sudo livecd-iso-to-disk --efi  --nomac --format --reset-mbr --overlay-size-mb 400000 ~/Downloads/Fedora-Workstation-Live-x86_64-40-1.14.iso /dev/sdc
```

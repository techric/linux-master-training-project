# Module 04: Advanced Storage, Filesystem, and Data Recovery Techniques (The Other 5%)

## Overview

This module covers advanced storage and filesystem diagnostics that are rarely used—until something goes very wrong. These skills become critical when:

- A filesystem becomes read-only  
- Disk performance drops unexpectedly  
- Inodes or space run out  
- LVM volumes fail  
- Corrupted files or corrupted metadata appear  
- Data needs to be recovered without rebooting  

This is high-risk, high-impact Linux work, and it separates intermediate engineers from senior ones.

---

## Learning Objectives

By the end of this module, you should be able to:

- Inspect block devices using `lsblk`, `blkid`, and `/proc/partitions`  
- Diagnose failing disks using `smartctl`  
- Understand filesystem corruption indicators  
- Check and repair filesystems with `fsck`  
- Analyze I/O bottlenecks with `iostat` and `iotop`  
- Inspect LVM metadata and recover volumes  
- Recover deleted files when possible  

---

## Lesson 1: Identifying Storage Devices

List block devices:
    lsblk

List block devices with filesystem info:
    lsblk -f

Show partition table:
    sudo fdisk -l

Show UUIDs and filesystem type:
    blkid

Low-level kernel info:
    cat /proc/partitions

These commands identify:

- Incorrectly attached volumes  
- Missing partitions  
- Filesystems not detected at boot  

---

## Lesson 2: Disk Health and SMART Diagnostics

Install smartmontools (if needed):
    sudo apt install smartmontools

Check SMART health:
    sudo smartctl -H /dev/sda

Full SMART report:
    sudo smartctl -a /dev/sda

Test the disk:
    sudo smartctl -t short /dev/sda
    sudo smartctl -t long  /dev/sda

Look for:

- Reallocated sectors  
- Pending sectors  
- High temperature  
- Uncorrectable errors  

SMART failures usually indicate a failing disk.

---

## Lesson 3: Filesystem Issues and Recovery

Check filesystem usage:
    df -h

Check inode usage:
    df -i

Identify filesystem corruption from logs:
    dmesg | grep -i fs
    dmesg | grep -i error

Unmount before checking:
    sudo umount /dev/sda1

Run fsck (only on unmounted or readonly filesystems):
    sudo fsck -f /dev/sda1

Common issues fsck fixes:

- Bad blocks  
- Corrupted metadata  
- Journaling errors  
- Orphaned inodes  

**Never run fsck on a mounted read-write filesystem.**

---

## Lesson 4: Recovering From Read-Only Filesystems

Linux may remount a filesystem read-only when errors occur.

Check mount status:
    mount | grep sda1

Remount attempt:
    sudo mount -o remount,rw /dev/sda1

If it fails:

1. Look at dmesg:
       dmesg | tail

2. Run fsck in maintenance mode.

3. If AWS/GCP/Azure:  
   - Detach volume  
   - Attach to rescue VM  
   - Run fsck safely  
   - Reattach  

This is common in cloud environments after sudden shutdowns.

---

## Lesson 5: I/O Performance Diagnostics

Install sysstat if needed:
    sudo apt install sysstat

I/O stats:
    iostat -xz 1

Look for:

- High await times  
- High svctime  
- High %util (close to 100%)  
- Queue depth issues  

Top processes doing I/O:
    sudo iotop

Check disk latency via:
    dmesg | grep -i "blk"

Symptoms of storage bottlenecks:

- Slow applications  
- Stalled writes  
- Database timeouts  
- Long fsync times  

---

## Lesson 6: LVM — Logical Volume Management (Advanced)

List volume groups:
    sudo vgdisplay

List logical volumes:
    sudo lvdisplay

List physical volumes:
    sudo pvdisplay

Scan for missing metadata:
    sudo lvmdiskscan

Activate a dormant or inactive VG:
    sudo vgchange -ay

Find missing PVs:
    sudo pvs

Restore LVM metadata (only if necessary):
    sudo vgcfgrestore vgname

LVM failures often cause boot issues or missing volumes.

---

## Lesson 7: Recovering Deleted Files (Best Effort)

If a file was deleted but the process still has it open:

1. Find the process:
       ps aux | grep app

2. Find the open file descriptor:
       ls -l /proc/PID/fd | grep deleted

3. Copy the file out:
       cp /proc/PID/fd/FD_NUMBER /root/recovered_file

This only works when:

- The process is still running  
- The file descriptor is still open  

If metadata is deleted, tools like `extundelete` can sometimes recover:

Unmount first:
       sudo umount /dev/sda1

Then:
       sudo extundelete /dev/sda1 --restore-all

---

## Lesson 8: Real-World Scenarios

### Scenario 1: Disk at 100% utilization

1. Identify processes:
       iotop

2. Inspect iostat:
       iostat -xz 1

3. Check dmesg for I/O errors:
       dmesg | grep -i i/o

---

### Scenario 2: Application complaining about no space but df -h shows free space

Check inodes:
       df -i

Resolution: delete small files or rotate logs.

---

### Scenario 3: Volume missing after reboot

Check LVM:
       sudo vgdisplay
       sudo lvdisplay

Activate group:
       sudo vgchange -ay

Mount manually:
       sudo mount /dev/vgname/lvname /mnt

---

## Lab Exercise

1. List block devices:
       lsblk -f

2. Identify filesystem type and UUID:
       blkid

3. Inspect SMART health:
       sudo smartctl -H /dev/sda

4. Check inode usage:
       df -i

5. Run iostat for 10 seconds:
       iostat -xz 1 10

6. View slab allocator stats (bonus):
       cat /proc/slabinfo | head

Document all findings.

---

## Quiz

1. What does `smartctl -H` tell you?  
2. Why should you not run fsck on a mounted read-write filesystem?  
3. How does inode exhaustion differ from disk space exhaustion?  
4. How can you recover a deleted file still held open by a process?  
5. What does `iostat -xz` help identify?  

---

## Notes and Observations

Use this section to document:

- Disk warnings in dmesg  
- I/O bottlenecks you observed  
- Any unusual SMART attributes  
- Filesystems that remounted read-only  
- Any volume activation steps required

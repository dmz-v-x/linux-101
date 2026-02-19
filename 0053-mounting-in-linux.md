## Mounting in Linux

### 1. Introduction — What is Mounting in Linux?

Mounting is one of the most fundamental Linux concepts.

In Linux:

> Storage devices are not usable until they are attached to the filesystem.

Mounting is the mechanism that performs this attachment.

Simple definition:

✔ Mounting = Connecting a storage device to the Linux directory tree

---

## PART 1 — The Linux Filesystem Tree

---

### 2. The Single Tree Design

Linux uses a **single unified directory structure**.

Everything begins at:

	/

Called:

✔ Root directory

Unlike Windows:

✔ No separate drive letters (C:, D:, etc.)

All devices integrate into this one tree.

---

### 3. Why Devices Need Mounting

When you plug in a disk:

✔ System detects hardware  
✘ Files not directly accessible  

Why?

✔ Linux does not read raw disks directly  
✔ It reads mounted filesystems only  

---

## PART 2 — What Mounting Actually Does

---

### 4. The Purpose of Mounting

Mounting:

✔ Makes device accessible  
✔ Connects filesystem → directory  

Example:

/dev/sdb1 → /mnt/usb

After mounting:

✔ `/mnt/usb` behaves like a normal folder

---

### 5. Critical Mental Model

Before mount:

✔ Device exists  
✘ Files invisible  

After mount:

✔ Files visible  
✔ Read/write operations enabled  

---

## PART 3 — Devices & Partitions

---

### 6. Understanding Device Naming

Linux represents disks as:

	/dev/sda
	/dev/sdb

Partitions:

	/dev/sda1
	/dev/sdb1

---

### 7. Why Partitions Matter

Each partition contains:

✔ A filesystem

Examples:

✔ ext4  
✔ xfs  
✔ ntfs  
✔ vfat  
✔ exFAT  

Mounting operates on partitions, not raw disks.

---

## PART 4 — Manual Mounting (Step-by-Step)

---

### 8. Scenario — Plugging in a USB Drive

Goal:

✔ Access USB manually

---

### 9. Step 1 — Identify the Device

Use:

	lsblk

Shows:

✔ Connected disks  
✔ Partitions  
✔ Mount status  

Look for:

✔ Newly attached device (e.g., /dev/sdb1)

---

### 10. Step 2 — Create Mount Point

Mount point = Folder

	sudo mkdir /mnt/usb

---

### 11. Step 3 — Mount the Device

	sudo mount /dev/sdb1 /mnt/usb

Now device is attached.

---

### 12. Step 4 — Access Files

	cd /mnt/usb
	ls

Works like normal directory.

---

### 13. Step 5 — Unmount Safely

	sudo umount /mnt/usb

Critical step before removal.

---

### 14. Why Unmounting is Essential

Prevents:

✔ Data corruption  
✔ Incomplete writes  
✔ Filesystem damage  

---

---

## PART 5 — Summary of Basic Workflow

---

| Step | Command | Purpose |
|------|----------|----------|
| Identify device | `lsblk` | Find disk |
| Create mount point | `mkdir /mnt/usb` | Create folder |
| Mount | `mount /dev/sdb1 /mnt/usb` | Attach device |
| Access | `cd /mnt/usb` | Use storage |
| Unmount | `umount /mnt/usb` | Detach safely |

---

---

## PART 6 — “But Ubuntu Mounts Automatically?”

---

### 15. Important Clarification

Even when automatic:

✔ Mounting STILL happens

Desktop environments simply perform it for you.

---

### 16. Why Learn Manual Mounting?

Real-world scenarios:

✔ Servers (no GUI)  
✔ Custom mount paths  
✔ Permission control  
✔ Filesystem failures  
✔ Cloud infrastructure  

---

### 17. Server Environments

Servers usually lack:

✔ Auto-mount tools

Manual mount is mandatory.

---

### 18. Custom Mount Points

Auto-mount paths may be:

	/media/username/device-name

Manual mounting allows cleaner structure:

	/mnt/data
	/logs
	/backups

---

### 19. Control & Permissions

Manual mounting enables:

✔ Read-only mounts  
✔ User access control  
✔ Security constraints  

---

### 20. Special Filesystems

Auto-mount may fail for:

✔ Corrupt filesystem  
✔ Rare filesystem type  

Manual specification required:

	mount -t <filesystem>

---

---

## PART 7 — Why Mounting Exists at All

---

### 21. Linux Cannot Use Raw Disks

Disk → Filesystem → Mount → Usable

Without mounting:

✔ Hardware visible  
✘ Filesystem inaccessible  

---

### 22. Think of Mounting as Integration

Mounting integrates:

✔ Storage → Directory tree

---

---

## PART 8 — Cloud Scenario (Extremely Important)

---

### 23. Mounting in Cloud Systems

In cloud:

✔ No physical USB drives  
✔ Virtual disks instead  

Example:

✔ AWS EBS volumes

---

## PART 9 — Mounting an EBS Volume (Step-by-Step)

---

### 24. Goal

✔ Attach 10 GB disk  
✔ Mount at `/mnt/data`

---

### 25. Step 1 — Create & Attach Volume (AWS)

✔ Create EBS volume  
✔ Same Availability Zone  
✔ Attach to EC2  

Appears as:

✔ /dev/sdf (AWS)  
✔ /dev/xvdf (Linux)

---

### 26. Step 2 — SSH into Instance

	ssh -i my-key.pem ubuntu@public-ip

---

### 27. Step 3 — Verify Disk

	lsblk

Identify:

✔ New unmounted disk

---

### 28. Step 4 — Format Disk

	sudo mkfs -t ext4 /dev/xvdf

Required before mounting.

---

### 29. Step 5 — Create Mount Point

	sudo mkdir /mnt/data

---

### 30. Step 6 — Mount Disk

	sudo mount /dev/xvdf /mnt/data

---

### 31. Step 7 — Verify Storage

	df -h /mnt/data

---

## PART 10 — Real DevOps Use Cases

---

✔ Docker storage  
✔ Log separation  
✔ Backups  
✔ Database storage  
✔ Snapshots  

Mounting is core infrastructure skill.

---

---

## PART 11 — Persistent Mounting with `/etc/fstab`

---

### 32. What is `/etc/fstab`?

fstab = File System Table

Defines:

✔ What to mount  
✔ Where to mount  
✔ How to mount  
✔ When to mount  

---

### 33. Why Use fstab?

✔ Auto-mount at boot  
✔ Permanent disks  
✔ Servers  
✔ Custom options  

---

## PART 12 — Stable Device Identification (UUID)

---

### 34. Why Not Use `/dev/sdb1`?

Device names can change.

UUIDs are stable.

---

### 35. Retrieve UUID

	sudo blkid

Example:

	/dev/sdb1: UUID="xxxx-xxxx"

---

## PART 13 — Configuring fstab Entry

---

### 36. Example Entry

UUID=xxxx-xxxx  /mnt/usb  ext4  defaults,nofail  0  2

---

### 37. Field Breakdown

✔ UUID → Device identity  
✔ Mount point → Attachment location  
✔ Filesystem → Type  
✔ Options → Behavior  
✔ Dump → Legacy  
✔ fsck order → Check priority  

---

### 38. Important Mount Options

**defaults** → Standard behavior  
**nofail** → Boot continues if missing  
**ro** → Read-only  
**noexec** → Disable execution  

---

---

## PART 14 — Testing fstab Safely

---

### 39. Validate Without Reboot

	sudo mount -a

If errors → Fix immediately

---

### 40. Why Testing is Critical

Incorrect fstab can:

✔ Break boot process

---

---

## PART 15 — Advanced Mounting Insights

---

### 41. Temporary vs Persistent Mounts

Manual mount → Temporary  
fstab → Persistent  

---

### 42. Mounting Read-Only

	mount -o ro device mountpoint

---

### 43. Viewing Mounted Filesystems

	mount
	df -h

---

### 44. Unmounting Busy Devices

If error:

✔ “target is busy”

Find processes:

	lsof +D /mnt/usb

---

---

## PART 16 — Mental Model for Mastery

---

### 45. Mounting is Not Optional

Automatic or manual:

✔ Always required

---

### 46. Filesystem Integration Concept

Device → Invisible  
Mount → Usable  

---

### 47. Why Mounting is Foundational

Mounting affects:

✔ Storage  
✔ Performance  
✔ Security  
✔ Reliability  
✔ System design  

---

### 48. Final Thoughts

Mounting is one of the most critical Linux skills.

It underpins:

✔ Server administration  
✔ Cloud infrastructure  
✔ DevOps workflows  
✔ Storage management  

Linux systems revolve around mounted filesystems.

Understanding this deeply unlocks true system-level competence.

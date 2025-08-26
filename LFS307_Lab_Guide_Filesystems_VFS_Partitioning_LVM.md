
# LFS307 — Hands‑On Lab Guide
## Linux Filesystems, VFS, Disk Partitioning & LVM (Step‑by‑Step)

> **Purpose:** Safe, reproducible labs you can run on a throwaway VM to master Linux filesystems, partitioning, and LVM—*without touching your real disks*.

---

## Prerequisites & Safety

- Use a **test VM** with `sudo` access. Do **NOT** run on production.
- Packages you may need (Debian/Ubuntu):
  ```bash
  sudo apt update
  sudo apt install -y lvm2 xfsprogs btrfs-progs parted gdisk util-linux
  ```
  (RHEL/CentOS):
  ```bash
  sudo dnf install -y lvm2 xfsprogs btrfs-progs parted gdisk util-linux
  ```
- We will use **loopback disks** (disk image files attached as `/dev/loopX`) so you **won’t risk real disks**.
- Commands that **modify storage** are prefixed with `sudo`. Read each step before running it.

---

## Lab Topology

We will create four loop “disks”:
- `diskA.img` (~1 GiB) → for partitioning + filesystems
- `diskB.img` (~1 GiB) → for LVM (PV)
- `diskC.img` (~1 GiB) → for LVM (PV) – used for growth/striping/mirroring
- `diskD.img` (~1 GiB) → for LVM migration (pvmove)

Common mount points:
- `/mnt/lab/ext4`, `/mnt/lab/xfs`, `/mnt/lab/lv_ext4`, `/mnt/lab/lv_xfs`, etc.

Create a workspace:
```bash
sudo mkdir -p /mnt/lab
cd /mnt/lab
```

---

## 0) Environment Setup — Create Safe Loop Disks

1) **Create sparse image files (~1 GiB each)**
```bash
sudo fallocate -l 1G /mnt/lab/diskA.img
sudo fallocate -l 1G /mnt/lab/diskB.img
sudo fallocate -l 1G /mnt/lab/diskC.img
sudo fallocate -l 1G /mnt/lab/diskD.img
```

> Alternative (works everywhere): `sudo dd if=/dev/zero of=/mnt/lab/diskA.img bs=1M count=0 seek=1024` (repeat for B/C/D).

2) **Attach images to loop devices**
```bash
sudo losetup -fP /mnt/lab/diskA.img
sudo losetup -fP /mnt/lab/diskB.img
sudo losetup -fP /mnt/lab/diskC.img
sudo losetup -fP /mnt/lab/diskD.img
```

3) **Verify loop devices**
```bash
lsblk -o NAME,MAJ:MIN,SIZE,TYPE,MOUNTPOINTS | grep -E 'loop|NAME'
sudo losetup -a
```

> You should see new devices like `/dev/loop10`, `/dev/loop11`, etc. Your numbers may differ.

> **Tip:** If you re-run labs, detach with `sudo losetup -D` (detaches all free loop devices) *only after unmounting everything and removing LVM usage*.

---

## 1) Explore VFS & Existing Mounts (Read‑Only)

1) **Check supported filesystem drivers**
```bash
cat /proc/filesystems
```

2) **See what’s mounted and types**
```bash
mount | column -t
df -hT
```

3) **Explore special (virtual) filesystems**
```bash
ls -ld /proc /sys /dev
ls -l  /proc /sys/class/block
```

**Key Takeaways**
- VFS unifies multiple FS types (ext4, xfs, tmpfs, proc, sysfs, …).
- `/proc` and `/sys` are *virtual* filesystems backed by the kernel, not disk.

---

## 2) Disk Partitioning on a Loop Device (GPT with `parted`)

We’ll partition **diskA** (find its loop device name first). Replace `/dev/loopX` below with your actual device for `diskA.img`.

1) **Identify device for diskA.img**
```bash
sudo losetup -a | grep diskA.img
# Example output: /dev/loop10: [0082]:12345 (/mnt/lab/diskA.img)
```

2) **Create a GPT with two partitions using `parted`**
```bash
DEV=/dev/loop10   # <-- change to your loop device for diskA
sudo parted -s "$DEV" mklabel gpt
sudo parted -s "$DEV" mkpart primary ext4 1MiB 50%
sudo parted -s "$DEV" mkpart primary xfs  50%  100%
sudo partprobe "$DEV"   # inform kernel
```

3) **Verify partitions**
```bash
lsblk "$DEV" -o NAME,SIZE,TYPE
sudo fdisk -l "$DEV"
```

You should now have: `loop10p1` and `loop10p2` (or `loop10p1/loop10p2` style depending on distro).

---

## 3) Create & Mount Filesystems (ext4 + XFS)

1) **Format the partitions**
```bash
P1=${DEV}p1
P2=${DEV}p2

sudo mkfs.ext4 -L LAB_EXT4 "$P1"
sudo mkfs.xfs  -L LAB_XFS  "$P2"
```

2) **Create mount points & mount**
```bash
sudo mkdir -p /mnt/lab/ext4 /mnt/lab/xfs
sudo mount "$P1" /mnt/lab/ext4
sudo mount "$P2" /mnt/lab/xfs

df -hT | grep -E '/mnt/lab/(ext4|xfs)'
```

3) **Write sample data & observe**
```bash
sudo sh -c 'echo "hello ext4" > /mnt/lab/ext4/hello.txt'
sudo sh -c 'echo "hello xfs"  > /mnt/lab/xfs/hello.txt'
ls -l /mnt/lab/ext4 /mnt/lab/xfs
```

> **Note:** XFS **cannot shrink**. ext4 can be shrunk offline. Both can grow.

---

## 4) Persistent Mounts with `/etc/fstab` (UUIDs)

1) **Get UUIDs**
```bash
sudo blkid "$P1" "$P2"
```

2) **Add to `/etc/fstab`** (use your UUIDs)
```bash
# Backup first
sudo cp /etc/fstab /etc/fstab.bak.$(date +%F)

# Edit
sudo nano /etc/fstab
```

Example lines (replace UUIDs):
```
UUID=<UUID-EXT4> /mnt/lab/ext4 ext4 defaults,nofail 0 2
UUID/<UUID-XFS>  /mnt/lab/xfs  xfs  defaults,nofail 0 2
```

3) **Test mounting logic without reboot**
```bash
sudo umount /mnt/lab/ext4 /mnt/lab/xfs
sudo mount -a
df -hT | grep -E '/mnt/lab/(ext4|xfs)'
```

---

## 5) LVM Basics — PV → VG → LV

We’ll build LVM on **diskB** and **diskC** loop devices.

1) **Identify loop devices for diskB and diskC**
```bash
sudo losetup -a | grep -E 'diskB|diskC'
# Set variables appropriately:
PV1=/dev/loop11   # diskB
PV2=/dev/loop12   # diskC
```

> **Tip:** You can use whole devices as PVs—no partition required for labs.

2) **Create PVs**
```bash
sudo pvcreate "$PV1" "$PV2"
sudo pvs
```

3) **Create VG (Volume Group) from both PVs**
```bash
sudo vgcreate vg_lab "$PV1" "$PV2"
sudo vgs
```

4) **Create two LVs: one ext4, one XFS**
```bash
sudo lvcreate -L 300M -n lv_ext4 vg_lab
sudo lvcreate -L 300M -n lv_xfs  vg_lab
sudo lvs -a -o +devices
```

5) **Make filesystems & mount**
```bash
sudo mkfs.ext4 /dev/vg_lab/lv_ext4
sudo mkfs.xfs  /dev/vg_lab/lv_xfs

sudo mkdir -p /mnt/lab/lv_ext4 /mnt/lab/lv_xfs
sudo mount /dev/vg_lab/lv_ext4 /mnt/lab/lv_ext4
sudo mount /dev/vg_lab/lv_xfs  /mnt/lab/lv_xfs
df -hT | grep -E '/mnt/lab/lv_'
```

6) **Write sample data**
```bash
sudo sh -c 'dd if=/dev/urandom of=/mnt/lab/lv_ext4/blob.bin bs=1M count=50 status=none'
sudo sh -c 'dd if=/dev/urandom of=/mnt/lab/lv_xfs/blob.bin  bs=1M count=50 status=none'
ls -lh /mnt/lab/lv_ext4 /mnt/lab/lv_xfs
```

---

## 6) Online Expansion — ext4 vs XFS

> For ext4 you can use `lvextend -r` to grow LV **and** filesystem automatically.
> For XFS you must use `xfs_growfs` (filesystem must be mounted).

1) **Grow ext4 LV by +200M (+ FS in one go)**
```bash
sudo lvextend -L +200M -r /dev/vg_lab/lv_ext4
df -h /mnt/lab/lv_ext4
```

2) **Grow XFS LV by +200M, then grow FS**
```bash
sudo lvextend -L +200M /dev/vg_lab/lv_xfs
# XFS grows while mounted (point to mount dir)
sudo xfs_growfs /mnt/lab/lv_xfs
df -h /mnt/lab/lv_xfs
```

3) **Attempting shrink (educational)**
- **ext4 shrink** requires unmount + fsck + resize2fs:
  ```bash
  sudo umount /mnt/lab/lv_ext4
  sudo e2fsck -f /dev/vg_lab/lv_ext4
  # shrink to 350M (ensure used space < 350M)
  sudo resize2fs /dev/vg_lab/lv_ext4 350M
  sudo lvreduce -L 350M /dev/vg_lab/lv_ext4
  sudo mount /dev/vg_lab/lv_ext4 /mnt/lab/lv_ext4
  df -h /mnt/lab/lv_ext4
  ```
- **XFS cannot shrink** (documented limitation).

---

## 7) LVM Snapshots (Copy‑on‑Write)

1) **Create snapshot of `lv_ext4`**
```bash
sudo lvcreate -s -L 150M -n lv_ext4_snap /dev/vg_lab/lv_ext4
sudo lvs -a -o +devices
```

2) **Make changes on origin**
```bash
sudo sh -c 'echo "AFTER-SNAPSHOT" >> /mnt/lab/lv_ext4/marker.txt'
tail -n1 /mnt/lab/lv_ext4/marker.txt || true
```

3) **Rollback using merge**
```bash
sudo umount /mnt/lab/lv_ext4
sudo lvconvert --merge /dev/vg_lab/lv_ext4_snap
# After merge completes, the origin LV contains snapshot state
sudo mount /dev/vg_lab/lv_ext4 /mnt/lab/lv_ext4
tail -n1 /mnt/lab/lv_ext4/marker.txt || echo "file absent (rolled back)"
```

> Ensure snapshot has enough space (`-L`) or it will become invalid on heavy writes.

---

## 8) Striping & Mirroring with LVM

**Requirements:** at least **two PVs** in the VG (`$PV1` and `$PV2`).

1) **Striped LV (RAID0‑like) across 2 PVs**
```bash
sudo lvcreate -i2 -I64 -L 400M -n lv_striped vg_lab
sudo mkfs.xfs /dev/vg_lab/lv_striped
sudo mkdir -p /mnt/lab/lv_striped
sudo mount /dev/vg_lab/lv_striped /mnt/lab/lv_striped
df -h /mnt/lab/lv_striped
```

2) **Mirrored LV (RAID1‑like)**
```bash
sudo lvcreate -m1 -L 200M -n lv_mirror vg_lab
sudo mkfs.ext4 /dev/vg_lab/lv_mirror
sudo mkdir -p /mnt/lab/lv_mirror
sudo mount /dev/vg_lab/lv_mirror /mnt/lab/lv_mirror
df -h /mnt/lab/lv_mirror
```

> **Note:** Mirroring consumes space for each copy. Monitor `vgs`, `lvs` carefully.

---

## 9) Replacing a Disk with `pvmove` (Data Migration)

We’ll simulate replacing `$PV1` by migrating data to `diskD` (`$PV3`).

1) **Create a PV on diskD and add to VG**
```bash
PV3=/dev/loop13   # adjust to your diskD loop device
sudo pvcreate "$PV3"
sudo vgextend vg_lab "$PV3"
sudo pvs
```

2) **Move data from the old PV to the new PV**
```bash
sudo pvmove "$PV1" "$PV3"
sudo pvs
sudo vgs -o +devices
```

3) **Remove old PV from VG**
```bash
sudo vgreduce vg_lab "$PV1"
sudo pvremove "$PV1"
sudo pvs
```

---

## 10) Thin Provisioning (Advanced, Optional)

1) **Create a thin pool (600M) and thin LV (1.2G over‑provisioned)**
```bash
sudo lvcreate -L 600M -T vg_lab/pool_thin
sudo lvcreate -V 1200M -T vg_lab/pool_thin -n lv_thin
```

2) **Format & mount**
```bash
sudo mkfs.ext4 /dev/vg_lab/lv_thin
sudo mkdir -p /mnt/lab/lv_thin
sudo mount /dev/vg_lab/lv_thin /mnt/lab/lv_thin
df -h /mnt/lab/lv_thin
sudo lvs -a
```

> **Caution:** Monitor pool usage with `lvs -a`. If the thin pool fills up, writes will fail.

---

## 11) (Optional) LUKS Encryption on an LV

1) **Create an LV for crypto**
```bash
sudo lvcreate -L 200M -n lv_crypto vg_lab
```

2) **Initialize LUKS & open**
```bash
sudo apt install -y cryptsetup || sudo dnf install -y cryptsetup
sudo cryptsetup luksFormat /dev/vg_lab/lv_crypto
sudo cryptsetup open /dev/vg_lab/lv_crypto lv_crypto_mapper
```

3) **Filesystem & mount**
```bash
sudo mkfs.ext4 /dev/mapper/lv_crypto_mapper
sudo mkdir -p /mnt/lab/lv_crypto
sudo mount /dev/mapper/lv_crypto_mapper /mnt/lab/lv_crypto
df -h /mnt/lab/lv_crypto
```

> To close: `sudo umount /mnt/lab/lv_crypto && sudo cryptsetup close lv_crypto_mapper`.

---

## 12) Performance & Maintenance Tips

- **Quick write test (non‑scientific):**
  ```bash
  sudo sh -c 'dd if=/dev/zero of=/mnt/lab/lv_xfs/test.img bs=16M count=16 oflag=direct status=progress'
  ```
- **Space usage by directory:**
  ```bash
  sudo du -sh /mnt/lab/* 2>/dev/null
  ```
- **TRIM/discard (on SSDs/thin pools):**
  ```bash
  sudo fstrim -v /mnt/lab/lv_xfs || true
  ```
  Avoid `discard` mount option unless you understand its performance implications; prefer periodic `fstrim`.

- **Health checks:**
  - ext4: `sudo e2fsck -f /dev/vg_lab/lv_ext4` (unmounted)
  - xfs:  `sudo xfs_repair -n /dev/vg_lab/lv_xfs` (unmounted; `-n` = no modify)

---

## 13) Cleanup (Tear Down Safely)

> Run only when you’re done with all labs.

1) **Unmount mounts (ignore errors)**
```bash
sudo umount -R /mnt/lab 2>/dev/null || true
```

2) **Close encrypted mapper if used**
```bash
sudo cryptsetup close lv_crypto_mapper 2>/dev/null || true
```

3) **Remove LVs / VG / PVs**
```bash
sudo lvremove -y vg_lab
sudo vgremove -y vg_lab
# Identify PV devices actually used in your session (e.g., /dev/loop11 /dev/loop12 /dev/loop13):
sudo pvremove -y /dev/loop11 /dev/loop12 /dev/loop13 2>/dev/null || true
```

4) **Detach loop devices**
```bash
# List first:
sudo losetup -a
# Detach individually (adjust numbers):
sudo losetup -d /dev/loop10 2>/dev/null || true
sudo losetup -d /dev/loop11 2>/dev/null || true
sudo losetup -d /dev/loop12 2>/dev/null || true
sudo losetup -d /dev/loop13 2>/dev/null || true
```

5) **Remove images (optional)**
```bash
sudo rm -f /mnt/lab/diskA.img /mnt/lab/diskB.img /mnt/lab/diskC.img /mnt/lab/diskD.img
```

---

## Reference Cheat‑Sheet

- **List block devices:** `lsblk -f`
- **Show mounts & types:** `df -hT`, `mount | column -t`
- **Partitioning:** `parted`, `fdisk`, `gdisk`
- **Make FS:** `mkfs.ext4`, `mkfs.xfs`, `mkfs.btrfs`
- **Check FS:** `e2fsck` (ext*), `xfs_repair` (XFS)
- **LVM:** `pvcreate`, `vgcreate`, `lvcreate`, `lvextend`, `lvreduce`, `pvmove`, `lvconvert --merge`
- **Growth:** `resize2fs` (ext*), `xfs_growfs` (XFS)
- **UUIDs:** `blkid`
- **fstab test:** `mount -a`

---

### What You’ve Learned
- VFS concepts and how Linux exposes disparate filesystems uniformly.
- Safe partitioning with GPT on loop devices.
- Creating/maintaining **ext4** and **XFS** filesystems; using `/etc/fstab` with UUIDs.
- Building **LVM**: PV → VG → LV, and performing **online growth**, **snapshots**, **striping**, **mirroring**.
- Disk replacement via **pvmove**, **thin provisioning**, and optional **LUKS** encryption.
- Cleanup and verification.

> Keep this file as your lab playbook. Duplicate the loop‑disk steps to re‑run the exercises anytime.


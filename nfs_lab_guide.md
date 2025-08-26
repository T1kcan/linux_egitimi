# NFS Hands-On Lab Guide

## 1. Introduction
Network File System (NFS) allows Linux/Unix machines to share directories over a network. This lab covers both server and client configuration.

---

## 2. NFS Server Setup

### 2.1 Install NFS Server
```bash
# Ubuntu/Debian
sudo apt update
sudo apt install -y nfs-kernel-server

# RHEL/CentOS/Oracle Linux
sudo yum install -y nfs-utils
```

### 2.2 Create a Shared Directory
```bash
sudo mkdir -p /srv/nfs_share
sudo chown nobody:nogroup /srv/nfs_share
sudo chmod 777 /srv/nfs_share
```

### 2.3 Configure Exports
```bash
sudo nano /etc/exports
```
Add:
```
/srv/nfs_share 192.168.1.0/24(rw,sync,no_subtree_check)
```
Apply changes:
```bash
sudo exportfs -a
sudo systemctl restart nfs-kernel-server
```

### 2.4 Open Firewall (if needed)
```bash
sudo ufw allow from 192.168.1.0/24 to any port nfs
```

---

## 3. NFS Client Setup

### 3.1 Install NFS Client Tools
```bash
# Ubuntu/Debian
sudo apt install -y nfs-common

# RHEL/CentOS/Oracle Linux
sudo yum install -y nfs-utils
```

### 3.2 Mount the NFS Share
```bash
sudo mkdir -p /mnt/nfs_client
sudo mount 192.168.1.100:/srv/nfs_share /mnt/nfs_client
```
Verify:
```bash
df -h
ls /mnt/nfs_client
```

### 3.3 Auto-Mount on Boot (Optional)
Add to `/etc/fstab`:
```
192.168.1.100:/srv/nfs_share  /mnt/nfs_client  nfs  defaults  0  0
```

---

## 4. Verification
```bash
echo "Hello from server" | sudo tee /srv/nfs_share/test.txt
ls /mnt/nfs_client
```

---

## 5. Cleanup
```bash
sudo umount /mnt/nfs_client
sudo systemctl stop nfs-kernel-server
```

✅ NFS server and client are configured and working.


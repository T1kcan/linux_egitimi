# Samba Hands-On Lab Guide

## 1. Introduction
Samba allows Linux machines to share directories using the SMB/CIFS protocol, enabling access from both Linux and Windows clients. This lab covers both server and client setup.

---

## 2. Samba Server Setup

### 2.1 Install Samba
```bash
# Ubuntu/Debian
sudo apt update
sudo apt install -y samba

# RHEL/CentOS/Oracle Linux
sudo yum install -y samba samba-client
```

### 2.2 Create a Shared Directory
```bash
sudo mkdir -p /srv/samba_share
sudo chown nobody:nogroup /srv/samba_share
sudo chmod 777 /srv/samba_share
```

### 2.3 Configure Samba Share
```bash
sudo nano /etc/samba/smb.conf
```
Add at the bottom:
```
[sambashare]
   path = /srv/samba_share
   browseable = yes
   read only = no
   guest ok = yes
```
Restart Samba:
```bash
sudo systemctl restart smbd
```

---

## 3. Samba Client Setup

### 3.1 Linux Client
```bash
sudo apt install -y cifs-utils   # Ubuntu/Debian
sudo yum install -y cifs-utils   # RHEL/CentOS
sudo mkdir -p /mnt/samba_client
sudo mount -t cifs //192.168.1.100/sambashare /mnt/samba_client -o guest
ls /mnt/samba_client
```

### 3.2 Windows Client
1. Press `Win + R` and type:
```
\\192.168.1.100\sambashare
```
2. Press Enter to access the shared folder.

---

## 4. Verification
```bash
echo "Hello Samba World" | sudo tee /srv/samba_share/test.txt
ls /mnt/samba_client
```

---

## 5. Cleanup
```bash
sudo umount /mnt/samba_client
sudo systemctl stop smbd
```

✅ Samba server and client are configured and ready for Linux and Windows file sharing.



NFS Server Installation on CentOS
--------------
```bash
yum install nfs-utils
```
--------------
Create Serving Directory:
```bash
mkdir /nfs
--------------
chmod -R 755 /nfs
chown nfsnobody:nfsnobody /nfs
```
Start NFS Server Processes:
```bash
--------------
# systemctl enable rpcbind
# systemctl enable nfs-server
# systemctl enable nfs-lock
# systemctl enable nfs-idmap
# systemctl start rpcbind
# systemctl start nfs-server
# systemctl start nfs-lock
# systemctl start nfs-idmap
--------------
nano /etc/exports
/nfs *(rw,sync,no_subtree_check,no_root_squash,no_all_squash,insecure)
--------------
systemctl restart nfs-server
--------------
firewall-cmd --permanent --zone=public --add-service=nfs
firewall-cmd --permanent --zone=public --add-service=mountd
firewall-cmd --permanent --zone=public --add-service=rpc-bind
firewall-cmd --reload
```
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

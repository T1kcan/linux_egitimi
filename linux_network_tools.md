# Mastering Linux Networking Tools

This guide covers commonly used Linux networking tools:  
`wget`, `curl`, `ftp`, `sftp`, `scp`, and `ssh`.

---

## 1. `wget` – Download Files from the Web

### Download a single file
```bash
wget http://example.com/file.zip
```

### Download and save with a different name
```bash
wget -O newfile.zip http://example.com/file.zip
```

### Resume an interrupted download
```bash
wget -c http://example.com/file.zip
```

### Download in background
```bash
wget -b http://example.com/file.zip
```

### Download entire website (mirror)
```bash
wget -m http://example.com/
```

---

## 2. `curl` – Transfer Data from or to a Server

### Fetch a webpage
```bash
curl http://example.com
```

### Save output to a file
```bash
curl -o file.html http://example.com
```

### Follow redirects
```bash
curl -L http://example.com
```

### Show response headers
```bash
curl -I http://example.com
```

### Send POST data
```bash
curl -d "username=user&password=pass" http://example.com/login
```

### Send JSON data with headers
```bash
curl -H "Content-Type: application/json" -d '{"name":"linux"}' http://example.com/api
```

### Download multiple files
```bash
curl -O http://example.com/file1.zip -O http://example.com/file2.zip
```

---

## 3. `ftp` – File Transfer Protocol (Legacy)

### Connect to an FTP server
```bash
ftp ftp.example.com
```

### Login with username and password
```bash
Name: user
Password: *****
```

### Basic FTP commands inside session
```bash
ls         # List files
cd dir     # Change directory
get file   # Download file
put file   # Upload file
bye        # Exit
```

### Non-interactive FTP download
```bash
ftp -n <<END_SCRIPT
open ftp.example.com
user myuser mypass
get file.txt
bye
END_SCRIPT
```

---

## 4. `sftp` – Secure File Transfer (over SSH)

### Connect to server
```bash
sftp user@remote_host
```

### Basic commands inside session
```bash
ls          # List files
cd dir      # Change directory
get file    # Download file
put file    # Upload file
mget *      # Download multiple files
bye         # Exit
```

### Non-interactive single command
```bash
sftp user@remote_host:/path/file.txt
```

---

## 5. `scp` – Secure Copy

### Copy file from local to remote
```bash
scp file.txt user@remote_host:/home/user/
```

### Copy file from remote to local
```bash
scp user@remote_host:/home/user/file.txt ./
```

### Copy directory recursively
```bash
scp -r mydir/ user@remote_host:/home/user/
```

### Use specific SSH port
```bash
scp -P 2222 file.txt user@remote_host:/path/
```

---

## 6. `ssh` – Secure Shell

### Connect to a remote server
```bash
ssh user@remote_host
```

### Connect with a specific port
```bash
ssh -p 2222 user@remote_host
```

### Run a command remotely
```bash
ssh user@remote_host "ls -l /var/log"
```

### Enable X11 forwarding (GUI applications)
```bash
ssh -X user@remote_host
```

### Use a private key for authentication
```bash
ssh -i ~/.ssh/id_rsa user@remote_host
```

### Copy public key to remote server (passwordless login)
```bash
ssh-copy-id user@remote_host
```

---

# Summary

- **File downloading**: `wget`, `curl`  
- **File transfer (legacy)**: `ftp`  
- **Secure transfer**: `sftp`, `scp`  
- **Remote login & execution**: `ssh`  

These tools are essential for **system administrators, developers, DevOps, and security engineers**.


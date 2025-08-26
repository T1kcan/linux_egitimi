# 🧪 Anacron Lab Guide: Running Missed Jobs After Reboot

## 📌 Goal
In this lab, you will learn how to use **Anacron** so that daily jobs are not missed even if the system is turned off at the scheduled time.  
When the system starts again, Anacron will catch up and run the missed jobs.

---

## 1️⃣ Step: Check if Anacron is Installed
Debian/Ubuntu:
```bash
dpkg -l | grep anacron
sudo apt install anacron -y
```

RHEL/CentOS:
```bash
rpm -qa | grep anacron
sudo yum install cronie-anacron -y
```

---

## 2️⃣ Step: Create a Test Script
We’ll create a script that writes the current date and time to a log file every time it runs.

```bash
sudo mkdir -p /opt/anacron-test
echo '#!/bin/bash' | sudo tee /opt/anacron-test/logtime.sh
echo 'date >> /opt/anacron-test/anacron.log' | sudo tee -a /opt/anacron-test/logtime.sh
sudo chmod +x /opt/anacron-test/logtime.sh
```

Test it:
```bash
/opt/anacron-test/logtime.sh
cat /opt/anacron-test/anacron.log
```

---

## 3️⃣ Step: Add Job to Anacrontab
Configure Anacron to run this script **every day**, **2 minutes after the system boots**.

```bash
sudo nano /etc/anacrontab
```

Add this line at the end:
```
1   2   logtime-job   /opt/anacron-test/logtime.sh
```

📌 Explanation:
- `1` → Run every 1 day.  
- `2` → Run 2 minutes after system startup.  
- `logtime-job` → Job identifier (for logs).  
- `/opt/anacron-test/logtime.sh` → The script to execute.  

---

## 4️⃣ Step: Enable Anacron Service
Make sure Anacron is enabled and running:
```bash
sudo systemctl enable --now anacron
sudo systemctl status anacron
```

---

## 5️⃣ Step: Test the Job
1. Clear the log file:
   ```bash
   sudo rm /opt/anacron-test/anacron.log
   ```

2. Reboot the system:
   ```bash
   sudo reboot
   ```

3. After ~2 minutes post-boot, check the log file:
   ```bash
   cat /opt/anacron-test/anacron.log
   ```

Expected output (example):
```
Mon Aug 26 10:05:34 +03 2025
```

---

## 6️⃣ Step: Run Jobs Manually (Optional)
You can trigger Anacron jobs manually:
```bash
sudo anacron -n -d
```

- `-n`: Skip the delay, run immediately.  
- `-d`: Show debug output.  

---

✅ With this lab, you can confirm that jobs are **not missed** even if the system was turned off at the scheduled time.

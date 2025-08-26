# Systemd Hands-On Lab Guide

## Introduction
`systemd` is the modern init system for Linux. It allows you to manage services, control startup processes, and monitor logs. This guide will teach you how to create, manage, and monitor a custom service.

---

## Table of Contents
1. Understanding systemd
2. Checking service status
3. Starting, stopping, restarting services
4. Enabling/disabling services at boot
5. Viewing service logs
6. Creating a custom service
7. Hands-On Lab Exercises

---

## 1. Understanding systemd

- `systemd` starts services at boot.
- It manages dependencies and service order.
- Logs are centralized with `journalctl`.

---

## 2. Checking Service Status

```bash
systemctl status nginx.service
```

- Shows if the service is active, inactive, or failed.
- Provides recent log messages.

---

## 3. Starting, Stopping, Restarting Services

```bash
sudo systemctl start nginx.service
sudo systemctl stop nginx.service
sudo systemctl restart nginx.service
sudo systemctl reload nginx.service  # reload config without restarting
```

---

## 4. Enabling/Disabling Services at Boot

```bash
sudo systemctl enable nginx.service
sudo systemctl disable nginx.service
```

---

## 5. Viewing Service Logs

```bash
journalctl -u nginx.service      # View all logs for the service
journalctl -f -u nginx.service   # Follow logs live
```

---

## 6. Creating a Custom Service

Assume we have an executable at `/usr/local/bin/myapp`.

### Step 1: Create a Service File

Create `/etc/systemd/system/myapp.service`:

```ini
[Unit]
Description=My Custom App
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/myapp
Restart=on-failure
User=nobody
Group=nogroup

[Install]
WantedBy=multi-user.target
```

### Step 2: Reload systemd

```bash
sudo systemctl daemon-reload
```

### Step 3: Start and Check Service

```bash
sudo systemctl start myapp.service
sudo systemctl status myapp.service
```

### Step 4: Enable at Boot

```bash
sudo systemctl enable myapp.service
```

### Step 5: View Logs

```bash
journalctl -u myapp.service -f
```

---

## 7. Hands-On Lab Exercises

1. Create a simple shell script `/usr/local/bin/helloworld.sh`:

```bash
#!/bin/bash
while true; do
    echo "Hello World: $(date)" >> /tmp/helloworld.log
    sleep 5
 done
```

Make it executable:
```bash
sudo chmod +x /usr/local/bin/helloworld.sh
```

2. Create a systemd service file `/etc/systemd/system/helloworld.service`:

```ini
[Unit]
Description=Hello World Demo Service
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/helloworld.sh
Restart=always
User=nobody

[Install]
WantedBy=multi-user.target
```

3. Reload systemd, start the service, and enable it at boot:

```bash
sudo systemctl daemon-reload
sudo systemctl start helloworld.service
sudo systemctl enable helloworld.service
```

4. Check status and follow logs:

```bash
sudo systemctl status helloworld.service
journalctl -f -u helloworld.service
```

5. Stop the service:
```bash
sudo systemctl stop helloworld.service
```

6. Try restarting and verify logs.

> This lab lets you experience creating, managing, and monitoring a service using systemd.


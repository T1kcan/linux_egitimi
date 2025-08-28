# Mastering Linux Networking

Linux networking provides a wide range of tools and commands for managing, troubleshooting, and configuring network connections.

---

## 1. Checking Network Configuration

### Show IP addresses (modern command)
```bash
ip addr show
```
or shorter:
```bash
ip a
```

### Show routing table
```bash
ip route show
```

### Show network interfaces (old command)
```bash
ifconfig
```

### Display active network connections
```bash
netstat -tulnp
```

### Show listening ports with process details
```bash
ss -tulnp
```

---

## 2. Network Testing Tools

### Test connectivity with ping
```bash
ping google.com
```

### Check DNS resolution
```bash
nslookup google.com
dig google.com
host google.com
```

### Trace network route
```bash
traceroute google.com
```

### Check open ports on a host
```bash
nc -zv google.com 80
```

### Test HTTP/HTTPS requests
```bash
curl -I https://example.com
wget --spider https://example.com
```

---

## 3. Managing Network Interfaces

### Bring an interface up or down
```bash
ip link set eth0 up
ip link set eth0 down
```

### Assign an IP address
```bash
ip addr add 192.168.1.100/24 dev eth0
```

### Remove an IP address
```bash
ip addr del 192.168.1.100/24 dev eth0
```

### Add a default gateway
```bash
ip route add default via 192.168.1.1
```

### Delete a default gateway
```bash
ip route del default
```

---

## 4. Network Monitoring

### Show network statistics
```bash
netstat -i
ss -s
```

### Monitor live connections
```bash
watch -n 1 ss -tulwn
```

### Bandwidth usage (requires `iftop`)
```bash
sudo iftop -i eth0
```

### Packet analysis (requires `tcpdump`)
```bash
sudo tcpdump -i eth0
```

### Capture packets on port 80
```bash
sudo tcpdump -i eth0 port 80
```

---

## 5. Firewall Management

### Check firewall status (UFW)
```bash
sudo ufw status
```

### Allow/deny ports (UFW)
```bash
sudo ufw allow 22/tcp
sudo ufw deny 80/tcp
```

### Using `iptables` (basic example)
```bash
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 80 -j DROP
```

### Using `firewalld`
```bash
sudo firewall-cmd --get-active-zones
sudo firewall-cmd --zone=public --add-port=8080/tcp --permanent
sudo firewall-cmd --reload
```

---

## 6. Advanced Networking

### Configure a static IP (temporary)
```bash
ip addr add 192.168.1.50/24 dev eth0
ip route add default via 192.168.1.1
```

### Create a virtual network interface (alias)
```bash
ip addr add 192.168.1.60/24 dev eth0 label eth0:1
```

### Enable IP forwarding (routing)
```bash
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
```

### Set up NAT (Masquerading)
```bash
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

### Add persistent routes (example)
```bash
sudo nano /etc/sysconfig/network-scripts/route-eth0
```
Add:
```
192.168.2.0/24 via 192.168.1.1
```

---

## 7. Useful Networking Files

- `/etc/hosts` → local hostname resolution  
- `/etc/resolv.conf` → DNS servers  
- `/etc/hostname` → system hostname  
- `/etc/network/interfaces` (Debian/Ubuntu legacy)  
- `/etc/sysconfig/network-scripts/` (RHEL/CentOS legacy)  

---

## 8. Practical Use Cases

### Find all listening services
```bash
ss -tulwn
```

### Check which process uses a port
```bash
sudo lsof -i :8080
```

### Flush all IP addresses from an interface
```bash
ip addr flush dev eth0
```

### Restart networking service (systemd-based)
```bash
sudo systemctl restart NetworkManager
```

---

# Summary

- Use `ip` instead of `ifconfig` (modern tool).  
- Use `ss` instead of `netstat` (faster and better).  
- Tools like `ping`, `traceroute`, `tcpdump`, `iftop` help with troubleshooting.  
- Firewalls: **UFW** (simple), **firewalld** (RHEL), **iptables/nftables** (advanced).  
- Key configuration files: `/etc/hosts`, `/etc/resolv.conf`, `/etc/network/*`.  

Linux networking is essential for **system administrators, DevOps engineers, and security professionals**.


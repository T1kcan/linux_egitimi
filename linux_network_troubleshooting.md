
# Linux Network Troubleshooting Guide (Basic to Advanced)

This guide covers the **most essential Linux commands and techniques** for troubleshooting network issues, from beginner to advanced levels.

---

## 1. Basic Network Connectivity Checks

### Ping
```bash
ping google.com
ping -c 4 8.8.8.8   # Send 4 packets
ping -i 0.5 1.1.1.1 # Change interval
```
- Tests if a host is reachable.

### Hostname & DNS Resolution
```bash
hostname        # Show system hostname
hostname -I     # Show all assigned IPs
nslookup google.com
dig google.com
```
- Verifies DNS resolution.

---

## 2. Interface and Routing Checks

### Show Interfaces & IPs
```bash
ifconfig        # Legacy
ip addr show    # Modern equivalent
ip link show
```
- Displays network interfaces.

### Routing Table
```bash
route -n        # Legacy
ip route show   # Modern equivalent
```
- Shows kernel routing table.

### Default Gateway
```bash
ip route | grep default
```

---

## 3. Port and Service Connectivity

### Telnet & Netcat
```bash
telnet google.com 80
nc -zv google.com 443
```
- Checks port connectivity.

### Nmap (Port Scanning)
```bash
nmap 192.168.1.1
nmap -p 22,80,443 192.168.1.1
nmap -A target.com
```
- Scans for open ports and services.

---

## 4. Packet Capture and Analysis

### Tcpdump
```bash
tcpdump -i eth0
tcpdump -i eth0 port 80
tcpdump -nn -i eth0 host 8.8.8.8
```
- Captures and inspects packets.

### Wireshark / Tshark
```bash
tshark -i eth0
```
- Advanced packet analysis.

---

## 5. Firewall & Security Checks

### Iptables / Firewalld
```bash
iptables -L -n -v
firewall-cmd --list-all
```
- Verifies firewall rules.

### SELinux (if enabled)
```bash
getenforce
setenforce 0   # Temporarily set to permissive
```

---

## 6. Performance and Bandwidth Monitoring

### Netstat & ss
```bash
netstat -tulnp
ss -tulnp
```
- Shows open ports and listening services.

### Iftop / Nload
```bash
iftop -i eth0
nload eth0
```
- Monitors real-time traffic.

### Iperf3 (Bandwidth Testing)
```bash
iperf3 -s              # Run as server
iperf3 -c server_ip    # Run as client
```

---

## 7. Advanced Network Debugging

### Traceroute / MTR
```bash
traceroute google.com
mtr google.com
```
- Traces the route to a destination.

### ARP Table
```bash
arp -n
ip neigh show
```
- Shows ARP cache.

### DNS Debugging with dig
```bash
dig google.com ANY
dig +trace google.com
```

---

## 8. Logs and System Analysis

### System Logs
```bash
journalctl -u NetworkManager
dmesg | grep eth0
```
- Finds NIC or driver issues.

### Syslog
```bash
tail -f /var/log/syslog
tail -f /var/log/messages
```

---

# ✅ Summary

- **Basic tools**: ping, nslookup, ifconfig/ip, route
- **Connectivity tools**: telnet, nc, nmap
- **Monitoring tools**: tcpdump, wireshark, iftop, nload
- **Performance tools**: iperf3, ss, netstat
- **Advanced debugging**: traceroute, mtr, dig, arp
- **Logs**: journalctl, syslog

This package should cover **most Linux networking troubleshooting scenarios** from beginner to advanced levels.

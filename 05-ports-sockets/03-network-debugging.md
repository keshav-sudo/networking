# 🛠 Network Debugging Tools

---

## 1. Essential Commands

### ping - Test connectivity
```bash
ping google.com
ping -c 5 192.168.1.1  # 5 packets only
```

### traceroute - Path to destination
```bash
traceroute google.com
traceroute -n google.com  # No DNS lookup
```

### netstat / ss - View connections
```bash
netstat -tulpn  # TCP/UDP listening ports
ss -tulpn       # Modern alternative
ss -tan state established  # Active connections
```

### curl - HTTP testing
```bash
curl -I https://api.example.com  # Headers only
curl -v https://api.example.com  # Verbose
curl -w "@time-format.txt" -o /dev/null -s https://example.com
```

---

## 2. DNS Debugging

```bash
# Basic lookup
nslookup google.com
dig google.com

# Trace DNS resolution
dig +trace google.com

# Check specific DNS server
dig @8.8.8.8 google.com

# Reverse lookup
dig -x 8.8.8.8
```

---

## 3. Port Checking

```bash
# Is port open?
nc -zv 192.168.1.1 80
nc -zv localhost 3000-3010  # Range

# What's using a port?
lsof -i :3000
fuser 3000/tcp
```

---

## 4. Packet Capture

```bash
# tcpdump basics
sudo tcpdump -i eth0
sudo tcpdump port 80
sudo tcpdump host 192.168.1.100
sudo tcpdump -w capture.pcap  # Save to file

# View in Wireshark
wireshark capture.pcap
```

---

## 5. Quick Troubleshooting Flow

```
Can't connect to service?

1. ping host     → Is host reachable?
2. telnet host port → Is port open?
3. curl http://host:port → Is service responding?
4. check firewall → Are rules blocking?
5. check logs   → What's the service saying?
```

---

*Keep these commands handy for debugging!*

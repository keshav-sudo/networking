# 📁 FTP, SFTP, and SSH

---

## 1. Quick Overview

| Protocol | Purpose | Security | Port |
|----------|---------|----------|------|
| **FTP** | File Transfer | ❌ Unencrypted | 21 |
| **SFTP** | Secure File Transfer | ✅ Encrypted (SSH) | 22 |
| **SSH** | Secure Shell (remote access) | ✅ Encrypted | 22 |

---

## 2. FTP - File Transfer Protocol

```
Client                           FTP Server
  │                                   │
  │ ── Connect to port 21 ──────────►│ (Control)
  │                                   │
  │ ← Authenticate ──────────────────│
  │    USER username                  │
  │    PASS password (plain text!)    │
  │                                   │
  │ ── Data transfer ────────────────│ (Port 20)
  │    LIST, GET, PUT                 │

⚠️ Problem: Username, password, data ALL unencrypted!
```

**Avoid FTP. Use SFTP instead.**

---

## 3. SSH - Secure Shell

```
Your Terminal                    Remote Server
      │                               │
      │ ── SSH Handshake ───────────►│
      │    (encrypted tunnel)         │
      │                               │
      │ ── Authenticate ────────────►│
      │    Key or password            │
      │                               │
      │ ═══ Encrypted session ══════ │
      │     Run commands, transfer    │
```

### SSH in Practice

```bash
# Connect to server
ssh user@192.168.1.100

# Connect with key file
ssh -i mykey.pem user@server.com

# Copy file to server
scp file.txt user@server:/path/

# Copy from server
scp user@server:/path/file.txt ./local/
```

---

## 4. SFTP - SSH File Transfer Protocol

```
SFTP = FTP-like interface OVER SSH tunnel

Everything encrypted!
Uses port 22 (same as SSH)
```

### SFTP Commands

```bash
# Connect
sftp user@server.com

# Inside SFTP:
ls              # List remote files
lls             # List local files
get file.txt    # Download
put local.txt   # Upload
cd /path        # Change remote dir
lcd /local      # Change local dir
exit
```

---

## 5. Comparison

| Feature | FTP | SFTP | SCP |
|---------|-----|------|-----|
| Encryption | No | Yes | Yes |
| Port | 21 | 22 | 22 |
| Resume transfer | Yes | Yes | No |
| Directory listing | Yes | Yes | No |
| Interactive | Yes | Yes | No |

---

## 6. SSH Key Authentication

```bash
# Generate SSH key pair
ssh-keygen -t ed25519 -C "your@email.com"

# Copy public key to server
ssh-copy-id user@server.com

# Now login without password!
ssh user@server.com
```

```
~/.ssh/id_ed25519      # Private key (NEVER share!)
~/.ssh/id_ed25519.pub  # Public key (copy to server)
```

---

## 7. Quick Summary

```
FTP:  Old, insecure - AVOID
SFTP: Secure FTP over SSH - USE THIS
SSH:  Encrypted remote shell access

All secure options use port 22

SSH Key Auth:
1. Generate key pair
2. Copy .pub to server
3. Login without password
```

---

## 8. Quiz

1. Company still uses FTP for file transfers. What's the risk?
2. `scp` vs `sftp` - when to use which?
3. SSH config file location? How to set alias?

<details>
<summary>Answers</summary>

1. **Credentials and data sent in plaintext!** Anyone on network can capture passwords and files. Switch to SFTP immediately.

2. **scp**: Quick one-time copy. **sftp**: Interactive session, browse files, multiple operations.

3. **~/.ssh/config**
```
Host myserver
    HostName 192.168.1.100
    User admin
    IdentityFile ~/.ssh/mykey
```
Now just: `ssh myserver`
</details>

---

*Next: [WebSockets](./09-websockets.md)*

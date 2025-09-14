# Wireshark FTP Protocol Filters Cheat Sheet

## Client Commands

### USER command (login username)
```wireshark
ftp.request.command == "USER"
```

### PASS command (login password)
```wireshark
ftp.request.command == "PASS"
```
### Reqeust passive FTP session
```wireshark
ftp.request.command == "PASV"
```

### LIST command (list directory contents)
```wireshark
ftp.request.command == "LIST"
```

### STOR command (upload a file)
```wireshark
ftp.request.command == "STOR"
```

### RETR command (download a file)
```wireshark
ftp.request.command == "RETR"
```

---

## Server Replies

### 230 response code (login successful)
```wireshark
ftp.response.code == 230
```

### 530 response code (login failed)
```wireshark
ftp.response.code == 530
```

### 150 response code (starting a data transfer)
```wireshark
ftp.response.code == 150
```

### 226 response code (transfer complete)
```wireshark
ftp.response.code == 226
```
### 227 response code (Entering Passive Mode)
```wireshark
ftp.response.code == 227
```
### The data channel traffic
```wireshark
ftp-data
```
---

## FTP Response Argument (server reply message)

### Response message containing "successful"
```wireshark
ftp.response.arg contains "successful"
```

### Response message containing "denied"
```wireshark
ftp.response.arg contains "denied"
```
---
## FTP Request Argument (arguments to the command)

### USER command with specific username (admin)
```wireshark
ftp.request.command == "USER" && ftp.request.arg == "admin"
```

### RETR command requesting specific file (report.pdf)
```wireshark
ftp.request.command == "RETR" && ftp.request.arg == "report.pdf"
```

### Any STOR command (upload attempt)
```wireshark
ftp.request.command == "STOR"
```
---

# OSI Model vs. Wireshark Layers

This diagram maps what you see in Wireshark to the OSI model layers.

```
+------------------------+---------------------------+
| OSI Layer              | Wireshark Example        |
+------------------------+---------------------------+
| Layer 7: Application   | File Transfer Protocol   |
|                        | (FTP)                    |
+------------------------+---------------------------+
| Layer 6: Presentation  | (Usually part of App in  |
|                        | Wireshark – not separate)|
+------------------------+---------------------------+
| Layer 5: Session       | (Usually part of App in  |
|                        | Wireshark – not separate)|
+------------------------+---------------------------+
| Layer 4: Transport     | Transmission Control      |
|                        | Protocol (TCP)           |
+------------------------+---------------------------+
| Layer 3: Network       | Internet Protocol (IPv4) |
+------------------------+---------------------------+
| Layer 2: Data Link     | Ethernet II              |
+------------------------+---------------------------+
| Layer 1: Physical      | Not shown in Wireshark   |
|                        | (bits on the wire)       |
+------------------------+---------------------------+
```

### Notes
- Wireshark doesn’t directly show Layer 1 (Physical). It only shows frame size, capture length, etc.  
- Layers 5 and 6 (Session & Presentation) are usually combined into Layer 7 (Application) in Wireshark analysis.  
- Example packet: Ethernet II → IPv4 → TCP → FTP.  

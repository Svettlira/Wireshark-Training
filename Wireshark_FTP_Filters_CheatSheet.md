# Wireshark FTP Control Channel Filters Cheat Sheet

## FTP Request Command (client → server)
Show specific commands sent by the client:

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

## FTP Response Code (server replies)

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

## Suggested Workflow for Investigation
1. Filter for all USER commands to see attempted usernames.  
2. Use `ftp.response.code == 530` to identify failed logins.  
3. Check `ftp.response.code == 230` for successful logins.  
4. Filter for `RETR` or `STOR` to identify file transfers.  
5. Switch to `ftp-data` filter to view the actual file contents.  

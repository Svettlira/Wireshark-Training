# Wireshark FTP Control Channel Filters Cheat Sheet

## FTP Request Command (client → server)
Show specific commands sent by the client:

```wireshark
ftp.request.command == "USER"
```

```wireshark
ftp.request.command == "PASS"
```

```wireshark
ftp.request.command == "LIST"
```
 # upload a file
```wireshark
ftp.request.command == "STOR"  
```
# download a file
```wireshark
ftp.request.command == "RETR"   
```

---

## FTP Request Argument (arguments to the command)

```wireshark
ftp.request.command == "USER" && ftp.request.arg == "admin"
```

```wireshark
ftp.request.command == "RETR" && ftp.request.arg == "report.pdf"
```

```wireshark
ftp.request.command == "STOR"
```

---

## FTP Response Code (server replies)

```wireshark
ftp.response.code == 230   # Successful login
```

```wireshark
ftp.response.code == 530   # Failed login
```

```wireshark
ftp.response.code == 150   # Starting a data transfer
```

```wireshark
ftp.response.code == 226   # Transfer complete
```

---

## FTP Response Argument (server reply message)

```wireshark
ftp.response.arg contains "successful"
```

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

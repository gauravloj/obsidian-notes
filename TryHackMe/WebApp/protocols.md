
## FTP commands:
```

- USER <username>
- PASS <password>
- STAT - more info
- SYST - system type
- PASV - switch to passive mode
- TYPE A - switch to ASCII file transfer mode
- TYPE I - switch to binary file transfer mode
- QUIT - end connection

```




  ## SMTP commands
- Example communication

```
helo telnet
250 bento.localdomain
mail from: 
250 2.1.0 Ok
rcpt to: 
250 2.1.5 Ok
data
354 End data with .
subject: Sending email with Telnet
Hello Frank,
I am just writing to say hi!             
.
250 2.0.0 Ok: queued as C3E7F45F06
quit
221 2.0.0 Bye
Connection closed by foreign host.
```


## pop3 

- user authenticates by providing his username `USER frank` and password `PASS D2xc9CgD`
- Using the command `STAT`, we get the reply +OK 1 179; based on [RFC 1939](https://datatracker.ietf.org/doc/html/rfc1939), a positive response to STAT has the format +OK nn mm, where _nn_ is the number of email messages in the inbox, and _mm_ is the size of the inbox in octets (byte). 
- The command `LIST` provided a list of new messages on the server, 
- and `RETR 1` retrieved the first message in the list

```
telnet 10.10.20.113 110
Trying 10.10.20.113...
Connected to 10.10.20.113.
Escape character is '^]'.
+OK 10.10.20.113 Mail Server POP3 Wed, 15 Sep 2021 11:05:34 +0300 
USER frank
+OK frank
PASS D2xc9CgD
+OK 1 messages (179) octets
STAT
+OK 1 179
LIST
+OK 1 messages (179) octets
1 179
.
RETR 1
+OK
From: Mail Server 
To: Frank 
subject: Sending email with Telnet
Hello Frank,
I am just writing to say hi!
.
QUIT
+OK 10.10.20.113 closing connection
Connection closed by foreign host.

```

  

## IMAP

- IMAP requires each command to be prefixed by a random string. Eg. `L1 LOGIN user pass`
- authenticate using `LOGIN username password`
- listed our mail folders using `LIST "" “*”`
- check messages in an inbox: `EXAMINE INBOX`

```

pentester@TryHackMe$ telnet MACHINE_IP 143
Trying MACHINE_IP...
Connected to MACHINE_IP.
Escape character is '^]'.

* OK [CAPABILITY IMAP4rev1 UIDPLUS CHILDREN NAMESPACE THREAD=ORDEREDSUBJECT THREAD=REFERENCES SORT QUOTA IDLE ACL ACL2=UNION STARTTLS ENABLE UTF8=ACCEPT] Courier-IMAP ready. Copyright 1998-2018 Double Precision, Inc.  See COPYING for distribution information.

c1 LOGIN frank D2xc9CgD

* OK [ALERT] Filesystem notification initialization error -- contact your mail administrator (check for configuration errors with the FAM/Gamin library)

c1 OK LOGIN Ok.
c2 LIST "" "*"
* LIST (\HasNoChildren) "." "INBOX.Trash"
* LIST (\HasNoChildren) "." "INBOX.Drafts"
* LIST (\HasNoChildren) "." "INBOX.Templates"
* LIST (\HasNoChildren) "." "INBOX.Sent"
* LIST (\Unmarked \HasChildren) "." "INBOX"
c2 OK LIST completed
c3 EXAMINE INBOX
* FLAGS (\Draft \Answered \Flagged \Deleted \Seen \Recent)
* OK [PERMANENTFLAGS ()] No permanent flags permitted
* 0 EXISTS
* 0 RECENT
* OK [UIDVALIDITY 631694851] Ok
* OK [MYRIGHTS "acdilrsw"] ACL
c3 OK [READ-ONLY] Ok
c4 LOGOUT
* BYE Courier-IMAP server shutting down
c4 OK LOGOUT completed
Connection closed by foreign host.

```



|**Protocol**|**TCP** **Port**|**Application(s)**|**Data Security**|
|---|---|---|---|
|FTP|21|File Transfer|Cleartext|
|HTTP|80|Worldwide Web|Cleartext|
|IMAP|143|Email (MDA)|Cleartext|
|POP3|110|Email (MDA)|Cleartext|
|SMTP|25|Email (MTA)|Cleartext|
|Telnet|23|Remote Access|Cleartext|
| FTP    | 21  | File Transfer                   | Cleartext |
| FTPS   | 990 | File Transfer                   | Encrypted |
| HTTP   | 80  | Worldwide Web                   | Cleartext |
| HTTPS  | 443 | Worldwide Web                   | Encrypted |
| IMAP   | 143 | Email (MDA)                     | Cleartext |
| IMAPS  | 993 | Email (MDA)                     | Encrypted |
| POP3   | 110 | Email (MDA)                     | Cleartext |
| POP3S  | 995 | Email (MDA)                     | Encrypted |
| SFTP   | 22  | File Transfer                   | Encrypted |
| SSH    | 22  | Remote Access and File Transfer | Encrypted |
| SMTP   | 25  | Email (MTA)                     | Cleartext |
| SMTPS  | 465 | Email (MTA)                     | Encrypted |
| Telnet | 23  | Remote Access                   | Cleartext |
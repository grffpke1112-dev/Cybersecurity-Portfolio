# Day 1 - Linux Security Lab Evidence

## 1. Environment

Operating System:

```text
Ubuntu 24.04.5 LTS

User:

grffpke1112

UID:

1000

Groups:

adm
sudo
docker
2. Network

Command:

ip route

Result:

default via 10.88.x.x dev eth0

Analysis:

The Linux host has a default route through the eth0 interface.

3. Listening Ports

Command:

sudo ss -tulpn

Important findings:

0.0.0.0:22
0.0.0.0:2222
*:970
*:980
*:981

SSH processes:

Port 22   -> sshd PID 74
Port 2222 -> sshd PID 78
4. Process Analysis

Commands:

ps -fp 74
ps -fp 78
pstree -p 1

SSH process:

PID 74
/usr/sbin/sshd -p 22

PID 78
/usr/sbin/sshd -f /etc/ssh/sshd_config_2222

Process relationship:

sshd
 └── SSH Session
      └── bash
5. Authentication Log

Log file:

/var/log/auth.log

Command:

sudo tail -n 30 /var/log/auth.log

Observed events:

Connection closed by authenticating user root
127.0.0.1
[preauth]

Analysis:

The connection was closed before authentication was completed.

The source address was 127.0.0.1, which is the local loopback address.

This event alone is not sufficient to determine that an attack occurred.

6. Successful SSH Authentication

Command:

sudo grep -E "Failed|Accepted|Invalid" /var/log/auth.log

Observed:

Accepted publickey for grffpke1112
from 127.0.0.1

Authentication method:

Public Key
ECDSA

Analysis:

A successful SSH public-key authentication was recorded for the current user.

The connection originated from 127.0.0.1.

7. Failed Authentication Investigation

Search terms:

Failed
Invalid

Result:

No typical Failed password or Invalid user events were observed in the inspected results.

Therefore, there is currently no evidence of a typical SSH password brute-force attempt in the inspected log entries.

8. Login History

Commands:

sudo last -n 10
sudo lastb -n 10

Observed login source:

127.0.0.1

No failed-login entries were displayed by lastb.

Security Investigation Chain
User
  ↓
Host
  ↓
IP
  ↓
Port
  ↓
Service
  ↓
Process
  ↓
Authentication
  ↓
Log
  ↓
Security Event
Day 1 Conclusion

The Linux environment was successfully inspected.

The investigation identified:

Linux OS information
User and group information
Network routing
Listening ports
SSH services
Process relationships
Authentication logs
Successful SSH authentication
Login history

The investigation did not identify a confirmed external SSH brute-force attack in the inspected records.

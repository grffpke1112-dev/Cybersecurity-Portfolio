# Cybersecurity-Portfolio
Linux Security Lab and Cybersecurity Learning Portfolio
# Day 1 - Linux Security Lab

## 學習目標

建立 Linux 資安實驗環境，並學習收集與分析：

- Linux OS information
- User information
- Network information
- Listening ports
- Processes
- Process tree
- SSH authentication logs
- Login history

## Environment

- OS: Ubuntu 24.04.5 LTS
- User: grffpke1112
- UID: 1000
- Groups: adm, sudo, docker
- Environment: Cloud Shell

## Network

Default route:

```bash
ip route
Result:

default via 10.88.x.x dev eth0
Listening Services
sudo ss -tulpn

Important findings:

0.0.0.0:22
0.0.0.0:2222
*:970
*:980
*:981

SSH:

Port 22   -> sshd PID 74
Port 2222 -> sshd PID 78
Process Analysis
ps -fp 74
ps -fp 78
pstree -p 1

Observed:

sshd
 └── SSH sessions
      └── bash

Log Analysis

Log:

/var/log/auth.log

Successful authentication:

Accepted publickey for grffpke1112
from 127.0.0.1

Observed authentication method:

ECDSA public key

Security Findings

SSH service is listening on ports 22 and 2222.
Successful SSH authentication was observed from 127.0.0.1.
No typical Failed password or Invalid user events were observed in the inspected results.
Several root authentication attempts were closed during the pre-authentication stage.
SSH configuration contained deprecated options.
Local sudo commands were recorded in auth.log.

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
What I Learned
How to identify Linux users and groups.
How to identify listening ports.
How to map ports to processes.
How to inspect process relationships.
How to analyze SSH authentication logs.
How to distinguish suspicious-looking events from confirmed attack evidence.

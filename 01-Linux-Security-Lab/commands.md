# Day 1 - Linux Security Lab Commands

## 1. User Information

```bash
whoami
id
Operating System
cat /etc/os-release
uname -a
Network
ip addr
ip route
Listening Ports
sudo ss -tulpn
Process
ps aux --sort=-%cpu | head -n 10
Process Tree
pstree -p 1
SSH Process
ps -fp 74
ps -fp 78
Log Files
sudo find /var/log -maxdepth 2 -type f | head -30
SSH Authentication Log
sudo tail -n 30 /var/log/auth.log
SSH Authentication Analysis
sudo grep -E "Failed|Accepted|Invalid" /var/log/auth.log | tail -n 20
Login History
sudo last -n 10
sudo lastb -n 10
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

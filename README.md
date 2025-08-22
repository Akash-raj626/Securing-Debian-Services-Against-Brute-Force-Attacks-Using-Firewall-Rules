# Securing-Debian-Services-Against-Brute-Force-Attacks-Using-Firewall-Rules
This project demonstrates how to secure a Debian-based system using iptables firewall rules, restrict service access, simulate a brute-force attack using Hydra, and analyze the effectiveness of the applied security measures.
# Project Overview 

- The goal of this project is to:
- Configure a firewall using iptables to allow only specific services.
- Perform network scanning using Nmap to identify open ports.
- Exploit the system via SSH using valid credentials.
- Launch a Hydra brute-force attack to test password strength.
- Harden the SSH service by updating configurations and firewall rules.
- Re-run the Hydra attack to verify the improvements in security.

# Tools Used:

- iptables → firewall configuration
- Nmap → network reconnaissance
- Hydra → SSH password brute-force testing
- SSH → secure remote login

# Environment Setup

- Target Machine: Debian 13
- Configured as a firewall
- Services allowed: HTTP (80), HTTPS (443), SSH (22 initially, later hardened to 2222), ICMP (ping)
- Attacker Machine: Kali Linux

# Steps Performed 

- sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
- sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
- sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
- sudo iptables -A INPUT -p icmp -j ACCEPT
- sudo iptables -A INPUT -j DROP

# Performed Nmap Scan

- From Kali Linux:
- nmap -sS 192.168.1.10
- Discovered open ports: 22 (SSH), 80 (HTTP), 443 (HTTPS).

# SSH Login to Debian

- Using valid credentials:
- ssh akash@192.168.1.10
- Successfully logged in, proving the service was accessible.

# Initial Hydra Attack

From Kali Linux:

- hydra -l akash -P passlist.txt ssh://192.168.1.10
- Successfully cracked the weak password.
- Verified login via SSH.

# Strengthened Security

- Made changes in /etc/ssh/sshd_config:

- Port 2222
- PermitRootLogin no
- MaxAuthTries 3

- Then restarted the SSH service:

- sudo systemctl restart ssh

- Updated firewall rule to allow port 2222:

- sudo iptables -A INPUT -p tcp --dport 2222 -j ACCEPT

# Second Hydra Attack (Failed)

- Retried the brute-force attack:

- hydra -l akash -P passlist.txt ssh://192.168.1.10:2222


# Result:
- Hydra failed to connect due to:
- Changed SSH port.
- Limited authentication attempts.
- Hardened firewall rules.

